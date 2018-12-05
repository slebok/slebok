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
    eval( T_0 , Tp_0 ,P).
```

and so on and so forth.

## Typing

![alt text](TAPL%20Fig.%2019-4.png "Typing")
