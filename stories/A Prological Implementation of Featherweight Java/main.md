# A Prological Treatise of Featherweight Java

This story tells how I reconstructed parser, evaluator and type checker of Featherweight Java (FJ) as described in TAPL, Sect. 19, in Prolog (SWI-Prolog, to be precise). The insights of this story, if any, come from observing
1. how the implementation reconstructs the semantics of the book's figures' specifying syntax and semantics of FJ, and 
1. how the implementation deviates from the specifications (and why).

My work was partly motivated by Guy Steele's talk ["It’s Time for a New Old Language"](https://www.youtube.com/watch?v=dCuZkaaou0Q).

## Syntax and Parser

### Original Syntax Specification

The syntax of FJ is given in TAPL, Fig. 19-1:

(include figure here)

Note that the grammar uses nonterminals and metavariables (the latter are introduced in the book's accompanying text, and here can be identified by the absence of corresponding productions).

There are four things I'd like to point out:
1. The overline notation replaces for the Kleene star found in other grammar specification languages, subject to conventions that, were they not detailed in the accompanying text, would need to be derived from another syntax specification of Java. That is, the grammar as is can only be interpreted using extra knowledge; alone it is insufficient to drive a parser.
1. The grammar for terms is left recursive; also, the left associativity of member access requires some special treatment.
1. The grammar for terms does not introduce parentheses, even though these are required for member access on cast expressions.
1. Fig. 19-1 really specifies two grammars, one for programs (including terms) and one for values. The language of values is a sublanguage of the language of terms (all values are terms syntactically).

All findings are justified by the primary use of the grammar: providing an inductive definition of the language (or, rather, its syntax trees) suited to serve the evaluation and typing rules. 

### Syntax and Parser in Prolog

A grammar that is also suitable for parsing is reconstructed as a Definite Clause Grammar (DCG) in Prolog as follows:

```prolog
'P'(program(P)) --> repeating('CL'(P)).
```

This (start) rule is implicit in TAPL. The (non-ground) term `program(P)` that is an argument to the start nonterminal `'P'` (quoted becaue in Prolog, unquoted identifiers starting with a capital letter are interpreted as variables) serves the construction of the syntax tree; the (root) node is a term of type `program`. (Unfortunately, Prolog is untyped and SWI-Prolog has no way of declaring structs.) `repeating` is a meta-predicate that implements the Kleene star for its argument, a nonterminal. In the above rule, it effects to `P` being unified with a list of nodes representing class definitions (possibly empty).

```prolog
'CL'(class(C, D, F, K, M)) -->
    keyword("class"),
    identifier(C), % class name
    keyword("extends"),
    identifier(D), % superclass name; 'C' in the original grammar!
    symbol("{"),
    repeating('F'(F)), % field declarations
    'K'(K), % constructor
    repeating('M'(M)), % method definitions
    symbol("}").
    
'F'(field(C, F)) -->
    identifier(C), % name of field type
    identifier(F), % field name
    symbol(";").
```
    
Deviating from the original grammar, the first rule uses different metavariables `C` and `D` (encoded by Prolog variables with corresponding names) instead of the single metaviable C in the original grammar. This is necessary because otherwise, the grammar would require classes to extend themselves. Also, it introduces a new non-terminal `'F'`, which is required to use `repeating` for accepting sequences of field declarations.

```prolog
'K'(constructor(C, X, SF, TF)) -->
    identifier(C), % name of class
    symbol("("),
    repeating(variable(X), ","), % formals ("variables")
    symbol(")"),
    symbol("{"),
    keyword("super"),
    symbol("("),
    repeating(identifier(SF), ","), % actuals (super fields)
    symbol(")"),
    symbol(";"),
    repeating(init(TF)), % initialization
    symbol("}").

init(init(F, X)) -->
    identifier("this"), % "this" is a variable, not a keyword
    symbol("."),
    identifier(F), % field name
    symbol("="),
    identifier(X), % var name
    keyword(";").
```

The rule for constructors (`'K'`) uses a variant of `repeating` specifying a separator. The new nonterminal `'init'` was introduced for the same reason as `'F'` above; note that, unlike the original grammar, it binds to the metavariable `TF` a list of pairs (where the original grammar binds a pair of lists to two metavariables f overlined).

```prolog
'M'(method(C, M, X, T)) -->
    identifier(C), % name of return type
    identifier(M), % method name
    symbol("("),
    repeating(variable(X), ","), % formals ("variables")
    symbol(")"),
    symbol("{"),
    keyword("return"),
    't'(T), % term
    symbol(";"),
    symbol("}").

variable(variable(C, X)) -->
    identifier(C), % name of type
    identifier(X). % variable name
```

Again, a new nonterminal (`variable`) was introduced just for the purpose of repetition.

```prolog
t(cast(C, T)) --> % cast
    symbol("("),
    identifier(C), % name of target type
    symbol(")"),
    t(T).
t(T) -->     % expanded to cater for left recursion
    e(E),    % and left associativity of member access
    m(E, T).
t(T) -->     % cater for parenthesized receivers (not in original FJ syntax)
    symbol("("),
    t(E),
    symbol(")"),
    m(E, T).

e(varaccess(X)) --> % variable
    identifier(X).
e(new(C, A)) --> % object creation
    keyword("new"),
    identifier(C),
    symbol("("),
    repeating(t(A), ","),
    symbol(")").

% member access terms
m(T, T) --> % no member access
    empty.

m(R, T) --> % field access
    symbol("."),
    identifier(F),
    m(faccess(R, F), T).
m(R, T) --> % method invocation
    symbol("."),
    identifier(M),
    symbol("("),
    repeating(t(A), ","),
    symbol(")"),
    m(minvoc(R, M, A), T).
```

The grammar rule for terms required more elaborate reworking, to account for left recursion (removed by introducing `e`, for elementary terms), the left associativity of member access (catered for by `m` with two arguments, where the first builds up a possible chain of member accesses and the second returns it). Also, I fitted in the parentheses for member access on cast expressions; I am not sure how this accounts for the parentheses used in the evaluation rules of TAPL %CHECKME.

```prolog
v(new(C, Vs)) -->
    keyword("new"),
    [C], % identifier(C) does not work in string generation mode
    symbol("("),
    repeating(v(Vs), ","),
    symbol(")").
```

The rule for values is not used for parsing values (recall that the syntax of values if covered by the syntax of terms), but for checking whether a term is a value (%CHECKME: are terms and values not from different domains? the syntactic and the semantic domain?). This is done by passing it the syntax tree of a term (and ignoring the string that this produces); see below.

## Evaluation

### Original Evaluation Rules

The evaluation rules of FJ are given in TAPL, Fig. 19-3:

![alt text](TAPL%20Fig.%2019-3.png "Evaluation")

They make use of the auxiliary definitions provided by Fig. 19-2 (which are also used by the typing rules; see below):

![alt text](TAPL%20Fig.%2019-2.png "Auxiliary")

The evaluation rules adhere to a small-step style, meaning that they are repeatedly applied until a value is produced or evaluation gets stuck. This evaluation loop is implemented by the Prolog clause

```prolog
eval(T, T, _) :- is_val(T), !. % term is ground -> the term is the value!
eval(T, V, P) :-
    step(T, Tp, P),
    eval(Tp, V, P).
```

where `is_val(T)` calls `phrase(v(T), _)` or lifts it over the members of `T` if `T` is a list. The third argument `P`holds the program in whose context the term `T` is evaluated to the value `V`. 

The evaluation rules themselves are implemented as follows.

```prolog
% E-ProjNew
step(faccess(new(C, Vs), F_i), V_i, P) :-
    is_val(Vs), !,
    fields(C, P, Fs),
    nth0(I, Fs, field(_, F_i)),
    nth0(I, Vs, V_i).
```

Here, the head of the rule makes sure that it can only be applied to field accesses on constuctor invocations, while the first subgoal makes sure that `Vs` is indeed a list of values, which is another precondition to rule application. The repeated invocation of `nth0` selects from `Vs`, the list of values passed to the constructor of `C`, the one assigned to the field named `F_i` (where correspondence is via position `I`). `fields` is an auxiliary function defined in TAPL Fig. 19-2; note that it depends of the program `P` (which is always implicit in TAPL).

```prolog
% E-InvkNew
step(minvoc(new(C, Vs), M, Us), V, P) :-
    is_val(Vs), is_val(Us), !,
    mbody(M, C, P, (Xs, T_0)),
    substitute([this|Xs], [new(C, Vs)|Us], T_0, V).

% E-CastNew
step(cast(D, new(C, Vs)), new(C, Vs), P) :-
    is_val(Vs), !,
    subtype(C, D, P).

% E-Field
step(faccess(T_0, F), faccess(Tp_0, F), P) :-
    \+ is_val(T_0), !,
    eval(T_0, Tp_0, P).
    
% E-Invk-Recv)
step(minvoc(T_0, M, Ts), minvoc(Tp_0, M, Ts), P) :-
    \+ is_val(T_0), !,
    eval(T_0, Tp_0, P).

% E-Invk-Arg
step(minvoc(V_0, M, Ts), minvoc(V_0, M, Tps), P) :-
    is_val(V_0), !, % for is_val(Ts), E-Invk-New applies
    select(T_i, Ts, Tp_i, Tps), \+ is_val(T_i), !, % will succeed; see above
    eval(T_i, Tp_i, P).

% E-New-Arg
step(new(C, Ts), new(C, Tps), P) :-
    select(T_i, Ts, Tp_i, Tps), \+ is_val(T_i), !, % all ground Ts caught by E-InvkNew
    eval(T_i, Tp_i, P).

% E-Cast
step(cast(C, T_0), cast(C, Tp_0), P) :-
    eval(T_0, Tp_0, P).
```

## Typing

![alt text](TAPL%20Fig.%2019-4.png "Typing")

```prolog
% T-Var
type(E, varaccess(X), C, _) :-
    memberchk(variable(C, X), E), !.

% T-Field
type(E, faccess(T0, Fi), Ci, P) :-
    type(E, T0, C0, P),
    fields(C0, P, Fs),
    memberchk(field(Ci, Fi), Fs).

% T-Invk
type(E, minvoc(T0, M, Ts), C, P) :-
    type(E, T0, C0, P),
    mtype(M, C0, P, (Ds -> C)), %TBD: move P to end
    type(E, Ts, Cs, P),
    subtype(Cs, Ds, P).

% T-New
type(E, new(C, Ts), C, P) :-
    fields(C, P, Fs),
    findall(D, member(field(D, _), Fs), Ds),
    type(E, Ts, Cs, P),
    subtype(Cs, Ds, P).

% T-UCast
type(E, cast(C, T0), C, P) :-
    type(E, T0, D, P),
    subtype(D, C, P), !.

% T-DCast
type(E, cast(C, T0), C, P) :-
    type(E, T0, D, P),
    subtype(C, D, P),
    C \= D, !.

% T-SCast
type(E, cast(C, T0), C, P) :-
    % C \<: D and D \<: C follow from above cuts
    type(E, T0, D, P),
    writeln("cross cast from ~w to ~w!", [D, C]).

% typing a list of terms (implicit in TAPL)
type(_, [], [], _).
type(E, [T|Ts], [C|Cs], P) :-
    type(E, T, C, P),
    type(E, Ts, Cs, P).

% Method Typing *
ok(method(C0, M, Xs, T0), C, P) :-
    findall(Ci, member(field(Ci, _), Xs), Cs),
    type([variable(C, this)|Xs], T0, E0, P),
    subtype(E0, C0, P),
    P = program(CL), memberchk(class(C, D, _, _, _), CL),
    override(M, D, (Cs -> C0), P).

% swap arguments to enable lifting over lists of methods using maplist
ok4all(C, P, M) :- ok(M, C, P).

% Class Typing *
ok(class(C, D, Fs, K, Ms), P) :-
    C \= D, \+ subtype(D, C, P), % anti-symmetry; not in TAPL, Fig. 19-4
    findall(init(F, F), member(field(_, F), Fs), Is), % TBD: init(F, F) corresponds to?
    K = constructor(C, Xs, Ss, Is),
    findall(field(Cx, Nx), member(variable(Cx, Nx), Xs), GFs),
    findall(field(_, S), member(S ,Ss), Gs), % type of S not checked???
    append(Gs, Fs, GFs),
    fields(D, P, Gs), %TBD: move P to end
    maplist(ok4all(C, P), Ms).

% swap arguments to enable lifting over lists of classes using maplist
ok4all(P, C) :- ok(C, P).

% Program Typing *
ok(P) :-
    P = program(Cs),
    maplist(ok4all(P), Cs).
```
