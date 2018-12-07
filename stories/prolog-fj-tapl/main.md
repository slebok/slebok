# A Prological Reconstruction of Featherweight Java from TAPL

This is the story of how I naively reconstructed a parser, evaluator and type checker for Featherweight Java (FJ) from Benjamin Pierce's book %LINKME:"Types and Programming Languages" (TAPL), Sect. 19, in [Prolog](../tools/Prolog.md) ([SWI-Prolog](http://www.swi-prolog.org/pldoc/man?predicate=select/4), to be precise). The insights of this story, if any, come from observing

1. how the implementation reconstructs the semantics of the book's figures' specifying syntax and semantics of FJ, and 
2. how and why the implementation differs from the specifications.

The work was partly motivated by Guy Steele's talk ["It’s Time for a New Old Language"](https://www.youtube.com/watch?v=dCuZkaaou0Q).

## Syntax and Parser

### Original Syntax Specification

The syntax of FJ is given in TAPL, Fig. 19-1, left:

```
CL ::= class C extends C {$~C~$ $~f~$; K $~M~$}

K  ::= C($~C~$ $~f~$) {super($~f~$); this.$~f~$=$~f~$;}

M  ::= C m($~C~$ $~x~$) {return t;}

t  ::= x
     | t.f
     | t.m($~t~$)
     | new C($~t~$)
     | (C) t

v  ::= new C($~v~$)
```

Note that the grammar uses nonterminals, or *metavariables*, that have no rules (namely *C*, *f*, and *m*); they expand to (or represent) identifiers (of classes, fields, and methods, resp.).

Here is a number of findings:

1. The overline notation replaces for the %LINKME:Kleene star found in other grammar specification languages, subject to conventions that, were they not detailed in the accompanying text, would need to be reconstructed from a more precise syntax specification of FJ. That is, the grammar as is can only be interpreted using extra knowledge, and therefore is insufficient to drive a standard parser generator.
2. Multiple occurrences of the same metavariable in the same rule may expand to different strings. For instance, the two occurrences of *C* in "class *C* extends *C*" may expand to (and represent) different class names. This can be concluded from assuming that metavariables take the role of the nonterminals of a @Vadim%LINKME:CFG.
3. The grammar for terms is left recursive; also, the left associativity of member access requires attention.
4. The term sublanguage does not introduce parentheses, even though these are required for member access on cast expressions.
5. Fig. 19-1 really specifies two grammars, one for programs (including terms) and one for values. The language of values is a sublanguage of the language of terms in the sense that all values are also terms syntactically.

All findings are justified by the primary use of the grammar: providing an inductive definition of the language (or, rather, its syntax trees) suited to serve the evaluation and typing rules. 

### Prological Syntax and Parser

A grammar specification that is also suitable for parsing is reconstructed as a Definite Clause Grammar (DCG) in Prolog as follows.

```prolog
'P'(program(P)) --> repeating('CL'(P)).
```

This (start) rule is implicit in TAPL. The (non-ground) term `program(P)` that is an argument to the start nonterminal `'P'` (quoted because in Prolog, unquoted identifiers starting with a capital letter are interpreted as variables) serves the construction of the syntax tree; the (root) node is a term of type `program`. (Unfortunately, Prolog is untyped and SWI-Prolog has no way of declaring structs.) `repeating` is a meta-predicate that implements the Kleene star for its argument, a nonterminal. In the above rule, it effects to `P` being unified with a list of nodes representing class definitions (possibly empty).

```prolog
'CL'(class(C, D, F, K, M)) -->
    keyword("class"),
    identifier(C), % class name
    keyword("extends"),
    identifier(D), % superclass name; 'C' in the original grammar!
    symbol("{"),
    repeating('F'(F)), % field declarations
    'K'(K), % konstructor
    repeating('M'(M)), % method definitions
    symbol("}").
    
'F'(field(C, F)) -->
    identifier(C), % name of field type
    identifier(F), % field name
    symbol(";").
```

The metavariable *C* from the original grammar (where it represents class names) translates to a logic variable. Deviating from what the original grammar seems to suggest, different logic variables `C` and `D` are introduced for the two occurrences of *C* in the original grammar. This is necessary because unlike for a metavariable in the syntax specification, multiple occurrences of a logic variable in the same rule express equality of whatever gets substituted for them. The rule `'CL'` also introduces a new non-terminal `'F'`, which is required so as to be able to use the metapredicate `repeating` for accepting sequences of field declarations.

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

The rule for constructors (`'K'`) uses a variant of `repeating` specifying a separator. The new nonterminal `init` was introduced for the same reason as `'F'` above; note that, unlike the original grammar, it binds to the variable `TF` a list of pairs (where the original grammar binds a pair of lists to two metavariables *f* overlined).

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

Again, a new nonterminal `variable` was introduced for the purpose of handling repetition.

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

The grammar rule for terms required more elaborate reworking, to account for left recursion (removed by introducing `e`, for elementary terms), the left associativity of member access (catered for by spending a second argument on `m` (where the first builds up a possible chain of member accesses and the second returns it at the end of the chain). Also, I fitted in the parentheses for member access on cast expressions; @Vadim%CHECKME:I am not sure how this accounts for the parentheses used in the evaluation rules of TAPL.

Note that the parser uses backtracking, and that the grammar is unambiguous.

```prolog
v(new(C, Vs)) -->
    keyword("new"),
    [C], % identifier(C) does not work in string generation mode
    symbol("("),
    repeating(v(Vs), ","),
    symbol(")").
```

The rule for values is not used for parsing values (recall that the syntax of values is covered by the syntax of terms), but for checking whether a term is a value (@Ralf%CHECKME: are terms and values not from different domains? the syntactic and the semantic domain?). This will be done by generating (or attempting to generate) a string from the syntax tree (which, in Prolog, is done by invoking the grammar with a ground parse tree and a variable sentence).

### Summary

The primary purpose of the grammar as provided by Fig. 19-1 of TAPL is to hint at a specification of an abstract syntax of FJ, on which the specifications of Figs. 19-2 through 4 rely. The above DCG makes this specification explicit, by defining the term structures (in the arguments of the rule heads) from which syntax trees are constructed. The occurrences of the metavariables representing identifiers in Fig. 19-1 (not the metavariables themselves) translate to logic variables in the DCG rules, which serve to insert the accepted identifiers literally in the syntax tree; all other (occurrences of) metavariables of Fig. 19-1 translate to nonterminals of the DCG (Prolog goals and subgoals).

## Extracting the Subtype Relation

The subtype relation defined by an FJ program is specified by its `extends` clauses, plus the fact that `Object` is the root of the hierarchy (it does not have a supertype). 

### Original Rules

The subtype relation is extracted from a program using the rules given in TAPL, Fig. 19-1, right. My observations:

1. The first rule, a fact (or axiom), states reflexivity of the subtype relation. It uses a metavariable *C* that is not linked to a concrete program. Differing from the syntax specification in Fig.19-1, left, here the two occurrences of the metavariable *C* stand for the same identifier (class name).
2. The second rule states transitivity of the subtype relation. The metavariables *C*, *D*, and *E* stand for different class names in the general case; it is unclear whether the rule covers the cases that two or all three are the same.
3. The third rule relates the subtype relation to a program. Again, it is unclear whether *C* and *D* stand for strictly different class names.
4. There is no rule stating the antisymmetry required for a subtype relation proper.

### Implementation in Prolog

The original subtype rules of Fig. 19-1 are not immediately accommodated by Prolog's operational semantics. Here is an amalgamation of the three rules into two: 

```prolog
subtype(C, C, _) :- !.
subtype(C, D, P) :-
    P = program(CL),
    memberchk(class(C, E, _, _, _), CL), !,
    subtype(E, D, P).
```

@{Ralf, Vadim}%CHECKME: Any idea how to make Prolog reflect the original specification more directly?

Note that the logic variables `C`, `D`, and `E` may be instantiated with the same class name. That is, the rules are a materialization of the assumption that *C*, *D*, and *E* in Fig. 19-1, right, may represent the same identifier.

```prolog
subtype([], [], _).
subtype([C|Cs], [D|Ds], P) :-
    subtype(C, D, P),
    subtype(Cs, Ds, P).
```

%CHECKME: the extension to lists of types (required where?) needs to be made explicit.

Note that the proof of `subtype(C, D, P)` may recur infinitely if the subtype relation is circular and hence not antisymmetric.

## Evaluation

### Original Evaluation Rules

The evaluation rules of FJ are given in Fig. 19-3 of TAPL:

![TAPL Fig. 19-3](TAPL%20Fig.%2019-3.png "Evaluation")

They make use of the auxiliary definitions provided by Fig. 19-2 (which are also used by the typing rules; see below):

![TAPL Fig. 19-3](TAPL%20Fig.%2019-2.png "Auxiliary")

The evaluation rules adhere to a small-step style, meaning that they are repeatedly applied until a value is produced or evaluation gets stuck. 

Again, there are some observations to be made:

1. Unlike for the syntax specification, multiple occurrences of the same metavariable in the same rule represent the same (meta)term. @Ralf%CHECKME: other word for metaterm? Note that "term" is ambiguous here, since TAPL uses it for expressions. This can be concluded from %FIXME: what? It follows that metavariables now correspond directly to logic variables.
2. The metavariables denoted by *t* overline etc. are implicitly indexed with indices ranging from 1 to *n*; the use of the same index *i* means that metavariables are selected from the same position of the corresponding sequences. Note that this presupposes that the sequences are of the same length, which may, but need not be, guaranteed by the syntax rules of FJ (it is only guaranteed where two sequences are accepted by the grammar as a list of pairs; e.g.: *C* overline *x* overline, which is accepted as `[variable(C, X)]`).
3. The rules and their order specify the evaluation order of subterms. For instance, for a term *t* = *t*_0.*m*(*t*_1, *t*_2), the evaluation order of the subterms *t*_0 through *t*_2 is from left to right. 

## Translation of Evaluation Rules to Prolog

The evaluation loop (implicit in TAPL) is implemented by the Prolog predicate

```prolog
eval(T, T, _) :- is_val(T), !. % term is ground -> the term is the value!
eval(T, V, P) :-
    step(T, Tp, P),
    eval(Tp, V, P).
```

where `is_val(T)` calls [`phrase(v(T), _)`](http://www.swi-prolog.org/pldoc/doc_for?object=phrase/2) either directly or lifts it over the members of `T` if `T` is a list, to check that the term `T` represents a value (@Ralf%CHECKME: Do we not have a metatyping problem here? Is T not *either* a term *or* a value?). The third argument `P` of `eval` holds the program in whose context the term `T` is evaluated to the value `V`.  Note that the evaluation loop terminates successfully when the input term has been reduced to a value, and fails when it arrives at a term that, although it is not a value, cannot be reduced further.

The evaluation rules themselves are implemented as follows.

```prolog
% E-ProjNew
step(faccess(new(C, Vs), F_i), V_i, P) :-
    is_val(Vs), !,
    fields(C, P, Fs),
    nth0(I, Fs, field(_, F_i)),
    nth0(I, Vs, V_i).
```

Here, the head of the rule makes sure that it can only be applied to field accesses on constructor invocations, while the first subgoal makes sure that the argument to the constructor invocation, `Vs`, is indeed a list of values, which is another precondition to rule application. The repeated invocation of [`nth0`](http://www.swi-prolog.org/pldoc/man?predicate=nth0/3) selects from `Vs`, the list of values passed to the constructor of `C`, the one assigned to the field named `F_i` (where correspondence is via position `I`). `fields` is the auxiliary function defined in TAPL Fig. 19-2 (see above); note that it depends of the program `P` (which is always implicit in TAPL).

```prolo
% E-InvkNew
step(minvoc(new(C, Vs), M, Us), V, P) :-
    is_val(Vs), is_val(Us), !,
    mbody(M, C, P, (Xs, T_0)),
    substitute([this|Xs], [new(C, Vs)|Us], T_0, V).
```

Here,  `mbody`is the auxiliary function defined in TAPL Fig. 19-2 and `substitute` is another auxiliary predicate.

```
% E-CastNew
step(cast(D, new(C, Vs)), new(C, Vs), P) :-
    is_val(Vs), !,
    subtype(C, D, P).

% E-Field
step(faccess(T_0, F), faccess(Tp_0, F), P) :-
    \+ is_val(T_0), !,
    eval(T_0, Tp_0, P).
    
% E-Invk-Recv
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

### Original Typing Rules

The typing rules of FJ are given in TAPL, Fig. 19-4:

![TAPL Fig. 19-4](TAPL%20Fig.%2019-4.png "Typing")

They make use of the auxiliary definitions provided by Fig. 19-2 (see above).

1. The environment is lean: It is comprised of the formals of a method, plus the variable *this*. The program, i.e., the environment for class (or type), field and method lookup, is implicit. (%CHECKME: how is this required for typing?)

The typing rules

```
% T-Var
type(E, varaccess(X), C, _) :-
    memberchk(variable(C, X), E), !.
```

I somehow lost track of how E gets filled!

```prolog
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

% Method Typing
ok(method(C0, M, Xs, T0), C, P) :-
    findall(Ci, member(field(Ci, _), Xs), Cs),
    type([variable(C, this)|Xs], T0, E0, P),
    subtype(E0, C0, P),
    P = program(CL), memberchk(class(C, D, _, _, _), CL),
    override(M, D, (Cs -> C0), P).

% swap arguments to enable lifting over lists of methods using maplist
ok4all(C, P, M) :- ok(M, C, P).

% Class Typing
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

% Program Typing
ok(P) :-
    P = program(Cs),
    maplist(ok4all(P), Cs).
```

# ToDos

@{Ralf, Vadim}:

Please vote: Should I try to introduce operators like '|-' and use ':' to make the Prolog rules resemble the inference rules of TAPL more closely?

Please vote: Should I introduce additional auxiliary functions to implement metavariable binding in the inference rules (e.g., use something like value_of_field_i(...) rather than nth0(...), nth0(...) in E-ProjNew)?

@Ralf: Your experience with supporting type safety arguments by analyzing the above rules from within Prolog would be greatly appreciated!

@Vadim: can I have an overline, please?
