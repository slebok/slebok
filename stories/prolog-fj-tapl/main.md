# A Prological Reconstruction of Featherweight Java from TAPL

This is the story of how I naively reconstructed a $@parser@$, $@evaluator@$ and $@typechecker@$ for $@Featherweight Java@$ (FJ) from $[Benjamin Pierce](bibtex:person/Benjamin_C_Pierce)$'s book %LINKME:"Types and Programming Languages" (TAPL), Sect. 19, in $@Prolog@$ ([SWI-Prolog](http://www.swi-prolog.org/pldoc/man?predicate=select/4), to be precise). The insights of this story, if any, come from observing

1. how the implementation reconstructs the semantics of the book's figures specifying the syntax and semantics of FJ, and 
2. how and why the implementation differs from the specifications.

The work was partly motivated by $[Guy Steele](bibtex:person/Guy_L_Steele_Jr)$'s talk ["It’s Time for a New Old Language"](https://www.youtube.com/watch?v=dCuZkaaou0Q).

## Syntax and Parser

### Original Syntax Specification

The syntax of FJ is given in TAPL, Fig. 19-1, left:

![TAPL Fig. 19-1](TAPL%20Fig.%2019-1.png "Syntax and Subtyping")

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

Note that the grammar uses $@nonterminals@nonterminal symbol$, or *metavariables*, without production rules (namely *C*, *f*, and *m*); they expand to (or represent) identifiers (of classes, fields, and methods, resp.).

Here is a number of findings:

1. The overline notation replaces for the $@Kleene star@Kleene_closure$ found in other grammar specification languages, subject to conventions that, were they not detailed in the accompanying text, would need to be reconstructed from a more precise syntax specification of FJ. That is, the grammar as is can only be interpreted using extra knowledge, and therefore is insufficient to drive a standard $@parser generator@$.
2. In line with metavariables taking the place of nonterminals, multiple occurrences of the same metavariable in the same rule may expand to different strings, or phrases. For instance, the two occurrences of *C* in "`class C extends C`" may expand to (and represent) different class names. This means that the metavariables of the syntax can strictly not be equated with the logic variables of Prolog.
3. Member access makes the grammar for terms $@left recursive@left recursion$; removal of left recursion needs to take into account the $@left associativity@$ of member access.
4. The term sublanguage does not introduce parentheses, which are required for member access on cast expressions (as used as an example in TAPL).
5. Fig. 19-1 really specifies two grammars, one for programs (including terms) and one for values. The language of values is a sublanguage of the language of terms in the sense that all values are also terms syntactically.

All findings are justified by the primary use of the grammar: providing an inductive definition of the language (or, rather, its syntax trees) suited to serve the evaluation and typing rules. 

### Prological Syntax and Parser

A grammar specification that is also suitable for parsing is reconstructed as a $@Definite Clause Grammar@definite clause grammar$ (DCG) in Prolog as follows.

#### Start Rule

```prolog
'P'(program(P)) --> repeating('CL'(P)).
```

This (start) rule is implicit in TAPL. The (non-ground) term `program(P)` that is an argument to the start nonterminal `'P'` (quoted because in Prolog, unquoted identifiers starting with a capital letter are interpreted as variables) serves the construction of the syntax tree; the (root) node is a term of type `program`. (Unfortunately, Prolog is untyped and SWI-Prolog has no way of declaring structs.) `repeating` is a meta-predicate that implements the Kleene star for its argument, a nonterminal. In the above rule, it effects to `P` being unified with a list of nodes representing class definitions (possibly empty).

#### Class Declarations

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

The metavariable $C$ from the original grammar (where it represents class names) translates to a logic variable needed to transfer the accepted identifier to the parse tree. Note that different logic variables `C` and `D` are introduced for the two occurrences of $C$ in the original grammar; this is necessary because unlike for a metavariable in the syntax specification, multiple occurrences of a logic variable in the same rule express equality of whatever gets substituted for them. The rule `'CL'` also introduces a new non-terminal `'F'`, which is required so as to be able to use the metapredicate `repeating` for accepting sequences of field declarations. Note that `repeating('F'(F))` instantiates `F` with a list of pairs `field(C, F)`, rather than a pair of list, as suggested by $~C~$ $~f~$ in the original grammar.

#### Constructor Declarations

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

The rule for constructors (`'K'`) uses a variant of `repeating` specifying `,` as separator. The new nonterminal `init` was introduced for the same reason as `'F'` above.

#### Method Declarations

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

Again, a new nonterminal `variable` was introduced for the purpose of handling a repeating group.

#### Terms

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

The grammar rule for terms required more elaborate reworking, to account for left recursion (removed by introducing `e`, for elementary terms) and the left associativity of member access (catered for by spending a second argument on `m` (where the first builds up a possible chain of member accesses and the second returns it at the end of the chain). @{Vadim,Ralf}%CHEKME: Is there a more elegant or standard way of dealing with this problem? Also, I fitted in the parentheses for member access on cast expressions; @Vadim%CHECKME:I am not sure how this accounts for the parentheses used in the evaluation rules of TAPL.

Note that the parser uses backtracking, and that the grammar is unambiguous.

#### Values

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

The primary purpose of the grammar as provided by Fig. 19-1 of TAPL is to hint at a specification of an abstract syntax of FJ, on which the specifications of Figs. 19-2 through 4 rely. The above DCG makes this specification explicit, by defining the term structures (in the arguments of the rule heads) from which syntax trees are constructed. The *occurrences of* the metavariables (not the metavariables themselves) representing identifiers in Fig. 19-1 (i.e., the ones without production rules) translate to logic variables in the DCG rules, which serve to insert the accepted identifiers literally in the syntax tree; all other (occurrences of) metavariables of Fig. 19-1 translate to nonterminals of the DCG (Prolog goals and subgoals).

## Extracting the Subtype Relation

The subtype relation defined by an FJ program is specified by its `extends` clauses, plus the fact that `Object` is the root of the hierarchy (it does not have a supertype). 

### Original Rules

The subtype relation is extracted from a program using the rules given in TAPL, Fig. 19-1, right. My observations:

@Vadim: If the original Figures are to be dropped, then we would need a transcription here.

1. The first rule, a fact (or axiom), states reflexivity of the subtype relation. It uses a metavariable $C$ that is not linked to a concrete program. Differing from the syntax specification in Fig.19-1, left, here the two occurrences of the metavariable $C$ stand for the same identifier (class name).
2. The second rule states transitivity of the subtype relation. The metavariables $C$, $D$, and $E$ stand for different class names in the general case; it is unclear whether the rule covers the cases that two or all three are the same.
3. The third rule relates the subtype relation to a program. Again, it is unclear whether $C$ and $D$ stand for strictly different class names.
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

Note that the proof of `subtype(C, D, P)` may recur infinitely if the subtype relation is circular (not antisymmetric).

@{Ralf, Vadim}%CHECKME: Any idea how to make Prolog reflect the original specification more directly?

Note that the logic variables `C`, `D`, and `E` may be instantiated with the same class name. That is, the rules are a materialization of the assumption that *C*, *D*, and *E* in Fig. 19-1, right, may represent the same identifier.

Some of the typing rules of Fig. 19-4 in TAPL require an extension of the subtype relation to lists of types (e.g., $~C~$ <: $~D~$). This is covered in Prolog by 

```prolog
subtype([], [], _).
subtype([C|Cs], [D|Ds], P) :-
    subtype(C, D, P),
    subtype(Cs, Ds, P).
```

## Evaluation

### Original Evaluation Rules

The evaluation rules of FJ are given in Fig. 19-3 of TAPL:

![TAPL Fig. 19-3](TAPL%20Fig.%2019-3.png "Evaluation")


```
   $/fields/$(C) = $~C~$ $~f~$
—————————————————————— (E-ProjNew)
  (new C($~v~$)).f$_i_$ --> v$_i_$  

            $/mbody/$(m,C) = ($~x~$, t$_0_$)
———————————————————————————————————————————————— (E-InvkNew)
(new C($~v~$)).m($~u~$) --> [$~x~$ |-> $~u~$, this |-> new C($~v~$)] t$_0_$

        C <: D
———————————————————————— (E-CastNew)
(D)(new C($~v~$)) --> new C($~v~$)

  t$_0_$ --> t'$_0_$
———————————— (E-Field)
t$_0_$.f --> t'$_0_$.f

      t$_0_$ --> t'$_0_$
——————————————————— (E-Invk-Recv)
t$_0_$.m($~t~$) --> t'$_0_$.m($~t~$)

         t$_i_$ --> t'$_i_$
———————————————————————————————— (E-Invk-Arg)
v$_0_$.m($~v~$, t$_i_$, $~t~$) --> v$_0_$.m($~v~$, t'$_i_$, $~t~$)

          t$_i_$ --> t'$_i_$
—————————————————————————————————— (E-New-Arg)
new C($~v~$, t$_i_$, $~t~$) --> new C($~v~$, t'$_i_$, $~t~$)

  t$_0_$ --> t'$_0_$
———————————— (E-Cast)
(C)t$_0_$ --> (C)t'$_0_$
```

They make use of the auxiliary definitions provided by Fig. 19-2 (which are also used by the typing rules; see below):

![TAPL Fig. 19-2](TAPL%20Fig.%2019-2.png "Auxiliary")

The evaluation rules adhere to a small-step style, meaning that they are repeatedly applied until a value is produced or evaluation gets stuck. 

Again, there are some observations to be made:

0. Matavariables now stand for subtrees of the program's (abstract) syntax tree.
1. Unlike for the syntax specification, multiple occurrences of the same metavariable in the same rule represent the same subtree (note that this holds for metavariables representing identifiers and nonterminals proper alike). It follows that in Figure 19-2, all metavariables correspond directly to logic variables.
0. Unambiguousness (or determinism) of rule selection depends on values ($v$) not being terms ($t$).
2. The metavariables denoted by *$~t~$* @Vadim%CHECKME: When to use *$$*, when just $$? etc. are implicitly indexed with indices ranging from 1 to *n*; the use of the same index *i* means that metavariables are selected from the same position of the corresponding sequences. Note that this presupposes that the sequences are of the same length, which may, but need not be, guaranteed by the syntax rules of FJ (it is only guaranteed where two sequences are accepted by the grammar as a list of pairs; e.g.: *$~C~$* *$~x~$*, which is accepted as `[variable(C, X)]`).
3. The success of evaluation either depends on the order in which the rules are visited and selected for application, or on the use of backtracking. For instance, the evaluation of $new C(new D()).f$ gets stuck if the matching rule E-Field is applied; it does not if E-ProjNew (which also matches and comes first) is applied instead (first or via backtracking). Determinisim of rule selection assumes a "select first match" policy.
4. The rules and their order specify the evaluation order of subterms. For instance, for a term *t* = *t*_0.*m*(*t*_1, *t*_2), the evaluation order of the subterms *t*_0 through *t*_2 is from left to right. @Vadim%CHECKME:$t_0$?

### Translation of Evaluation Rules to Prolog

#### Evaluation Loop

The evaluation loop (implicit in TAPL) is implemented by the Prolog predicate

```prolog
eval(T, T, _) :- is_val(T), !. % term is ground -> the term is the value!
eval(T, V, P) :-
    step(T, Tp, P),
    eval(Tp, V, P).
```

where `is_val(T)` calls [`phrase(v(T), _)`](http://www.swi-prolog.org/pldoc/doc_for?object=phrase/2) either directly or lifts it over the members of `T` if `T` is a list, to check that the term `T` represents a value (@Ralf%CHECKME: Do we not have a metatyping problem here? Is T not *either* a term *or* a value?). The third argument `P` of `eval` holds the program in whose context the term `T` is evaluated to the value `V`.  Note that the evaluation loop terminates successfully when the input term has been reduced to a value, and fails when it arrives at a term that, although it is not a value, cannot be reduced further.

The cut is inserted to satisfy the testing framework; it eliminates choicepoints. However, all choicepoints should eventually lead to failure. %CHECKME which is why the cuts should be removed, so that the latter can be shown by testing.

The evaluation rules themselves are implemented as follows.  Note that the syntactic forms (@Ralf%CHECKME: better term?) of the original rules have been replaced by terms coding the types of the corresponding nodes of the (abstract) syntax tree. @Ralf%CHECKME: If possible, should I try to retain the original syntactic form? The generality of such a venture would depend on all syntactic forms always be representable in Prolog syntax. I am kind of scared by the problems this might cause.

Note that all cuts have been inserted only to remove vain choicepoints.

#### E-ProjNew

```prolog
step(faccess(new(C, Vs), F_i), V_i, P) :-
    is_val(Vs), !,
    fields(C, P, Fs),
    nth1(I, Fs, field(_, F_i)),
    nth1(I, Vs, V_i).
```

Here, the head of the rule makes sure that it can only be applied to field accesses on constructor invocations, while the first subgoal makes sure that the argument to the constructor invocation, `Vs`, is indeed a list of values, which is another precondition to rule application. The repeated invocation of [`nth1`](http://www.swi-prolog.org/pldoc/man?predicate=nth1/3) selects from `Vs`, the list of values passed to the constructor of `C`, the one assigned to the field named `F_i` (where correspondence is via position `I`). `fields` is the auxiliary function defined in TAPL Fig. 19-2 (see above); note that it depends of the program `P` (which is always implicit in TAPL).

#### E-InvkNew

```prolog
step(minvoc(new(C, Vs), M, Us), V, P) :-
    is_val(Vs), is_val(Us), !,
    mbody(M, C, P, (Xs, T_0)),
    substitute([this|Xs], [new(C, Vs)|Us], T_0, V).
```

Like above. Here,  `mbody` is the auxiliary function defined in TAPL Fig. 19-2 and `substitute` implements substitution.

#### E-CastNew

```prolog
step(cast(D, new(C, Vs)), new(C, Vs), P) :-
    is_val(Vs), !,
    subtype(C, D, P).
```

#### E-Field

```prolog
step(faccess(T_0, F), faccess(Tp_0, F), P) :-
    \+ is_val(T_0), !,
    eval(T_0, Tp_0, P).
```

The negative condition `\+ is_val(T_0)` removes the dependence on evaluation order. %CHECKME could be omitted then? If so, omit?

#### E-Invk-Recv

```prolog
step(minvoc(T_0, M, Ts), minvoc(Tp_0, M, Ts), P) :-
    \+ is_val(T_0), !,
    eval(T_0, Tp_0, P).
```
%CHECKME: removing negative condition causes infinite recursion!

#### E-Invk-Arg

```prolog
step(minvoc(V_0, M, Ts), minvoc(V_0, M, Tps), P) :-
    is_val(V_0), !, % for is_val(Ts), E-Invk-New applies
    select(T_i, Ts, Tp_i, Tps), \+ is_val(T_i), !, % will succeed; see above
    eval(T_i, Tp_i, P).
```

The [select](http://www.swi-prolog.org/pldoc/man?predicate=select/4) predicate replaces the first occurrence of `T_i` in `Ts` with `Tp_i`, yielding `Tps`. The condition `\+ is_val(T_i)` makes sure (via backtracking into `select`) that all preceeding arguments are values, as requested by the original rule. The cut is required to make the choice of `T_i` deterministic; selection of a `T_i` will succeed because the case that all arguments are values is caught by E-InvkNew.

#### E-New-Arg

```prolog
step(new(C, Ts), new(C, Tps), P) :-
    select(T_i, Ts, Tp_i, Tps), \+ is_val(T_i), !, % all ground Ts caught by E-InvkNew
    eval(T_i, Tp_i, P).
```

like above

#### E-Cast

```prolog
step(cast(C, T_0), cast(C, Tp_0), P) :-
    eval(T_0, Tp_0, P).
```

trivial

### Summary

The translation of the evaluation rules to Prolog is mostly straightforward, especially given that the original evaluation procedure assumes an SLD resolution like approach. The helper predicates make some of the implicitness in the original notation explicit. The distinction of values and terms is made explicit. @Ralf%CHECKME: Unclear to me whether terms and values are sufficiently separated in the original capture.

## Typing

### Original Typing Rules

The typing rules of FJ are given in TAPL, Fig. 19-4:

![TAPL Fig. 19-4](TAPL%20Fig.%2019-4.png "Typing")

They make use of the auxiliary definitions provided by Fig. 19-2 (see above).

My observations:
0. The interpretation of metavariables as logic variables is mandatory.
1. Types are noting but symbols (class names).
2. The typing environment is lean: it is set up by the rule method type checking (`M OK in C`) and comprised of the formals of a method, plus the variable *this*. The environment for field and method type lookup is implicit (declarations are read from the program, which is also implicit; see above).

Note that the so-called stupid cast is elsewhere also known as a cross cast. It makes perfect sense in Java with interfaces (where expressions may be type cast from one interface to a sibling interface).

### Translation of the Typing Rules to Prolog

The `type` predicate implementing the type judgements has four arguments:
- E, the typing environment, which is a list of terms `variable(X, C)`;
- a pattern for the syntactic category to be typed; @Ralf%CHECKME: where does the term "syntactic category" fit in all of the above?
- C, the type represented by a class name; and
- P, the program, which is required for lookups of names via `fields` and `mtype` (implicit in TAPL, which is why it comes last even though it is strictly an input).
 
Note that the names of variables largely follow those used in TAPL; a plural-s replaces for overline.
 
 %CHECKME consider rewriting predicates using operators '|-', ':', and '<:'

#### T-Var

```prolog
type(E, varaccess(X), C, _) :-
    memberchk(variable(C, X), E), !.
```

a (deterministic) lookup of the type of a variable as used by a variable access %CHECKME: Cut only for testing?

#### T-Field

```prolog
type(E, faccess(T_0, F_i), C_i, P) :-
    type(E, T_0, C_0, P),
    fields(C_0, P, Fs),
    memberchk(field(C_i, F_i), Fs).
```

Note how the original rule operates on a pairs of lists ($~C~$ $~f~$), where the Prolog rule operates on a list of pairs (`[field(C_i, F_i)]`). In a more faithful reconstruction of the original rule (using pairs of lists), an explicit index variable `I` would be required (as in the above rule E-ProjNew, which implements extraction from a pair of lists). Maybe the translation to Prolog could be made more homogenous by using pairs of lists everywhere; however, this would need some working on the grammar, especially the implementation of `repeating` (or perhaps a reimplementation of fields suffices!). @Ralf%CHECKME: Should I do this? 

#### T-Invk

```prolog
type(E, minvoc(T_0, M, Ts), C, P) :-
    type(E, T_0, C_0, P),
    mtype(M, C_0, P, (Ds -> C)), %TBD: move P to end
    type(E, Ts, Cs, P),
    subtype(Cs, Ds, P).
```
straightforward translation

#### T-New

```prolog
type(E, new(C, Ts), C, P) :-
    fields(C, P, Fs),
    findall(D, member(field(D, _), Fs), Ds),
    type(E, Ts, Cs, P),
    subtype(Cs, Ds, P).
```

straightforward, exept for the extraction of `Ds`, a list of types, from `Fs`, a list of pairs `[field(D, _)]` (see above)

##### T-UCast

```prolog
type(E, cast(C, T_0), C, P) :-
    type(E, T_0, D, P),
    subtype(D, C, P), !.
```

straightforward

#### T-DCast
```prolog
type(E, cast(C, T_0), C, P) :-
    type(E, T_0, D, P),
    subtype(C, D, P),
    C \= D, !.
```

straightforward %CHECKME: why cut?

#### T-SCast

```prolog
type(E, cast(C, T_0), C, P) :-
    % C \<: D and D \<: C follow from above cuts @Ralf%CHECKME: Should I strive for cutless rules?
    type(E, T_0, D, P),
    writeln("cross cast from ~w to ~w!", [D, C]).
```

straightforward; warning is printed on console

#### Typing a List of Terms
```prolog
type(_, [], [], _).
type(E, [T|Ts], [C|Cs], P) :-
    type(E, T, C, P),
    type(E, Ts, Cs, P).
```

This is implicit in TAPL.

#### Method Typing

```prolog
ok(method(C_0, M, Xs, T_0), C, P) :-
    findall(C_i, member(field(C_i, _), Xs), Cs),
    type([variable(C, this)|Xs], T_0, E_0, P),
    subtype(E_0, C_0, P),
    P = program(CLs), memberchk(class(C, D, _, _, _), CLs),
    override(M, D, (Cs -> C_0), P).

% swap arguments to enable lifting over lists of methods using maplist
ok4all(C, P, M) :- ok(M, C, P).
```

It finally gets ugly. `findall` extracts the list $~C~$ from the list of formals of the method (a list of pairs), needed for the auxilliary predicate `override`. The typing environment required for typing the method body `T_0` in the next line is again a list of pairs. `memberchk(class(C, D, _, _, _), CLs)` extracts the superclass of $C$ from the program. %CHECKME: How is the superclass of Object determined?; `override` implements the corresponding auxilliary function of Fig. 19-2.

#### Class Typing

```prolog
ok(class(C, D, Fs, K, Ms), P) :-
    C \= D, \+ subtype(D, C, P), % anti-symmetry; not in TAPL, Fig. 19-4
    findall(init(F, F), member(field(_, F), Fs), Is),
    K = constructor(C, Xs, Ss, Is),
    findall(field(Cx, Nx), member(variable(Cx, Nx), Xs), GFs),
    findall(field(_, S), member(S ,Ss), Gs), % type of S not checked???
    append(Gs, Fs, GFs),
    fields(D, P, Gs), %TBD: move P to end
    maplist(ok4all(C, P), Ms).

% swap arguments to enable lifting over lists of classes using maplist
ok4all(P, C) :- ok(C, P).
```

The first line adds some necessary well-typedness checks not made explicit in TAPL's Fig. 19-4. The second line constructs the initializations `Is` expected in the body of the constructor of the class, in the original rule given by $this.~f~=~f~$ (which uses a pair of lists or, rather, the same list twice; note how $this.$ and $=$ serve as indicators of the syntactic category, replaced by terms in the Prolog translation). `init(F, F)` expresses that the init expressions of the constructor of a class use identical names for fields and formals; this condition, which is a naming convention of FJ rather than a type constraint, will be enforced only in the next line, where it is unified with the init expressions found in the actual constructor. (Note that it cannot be enforced by the syntax specification, since its metavariables are strictly not logic variables.) 

The next `findall` creates `GFs`, the list of all fields that should be inherited and declared by class `C`, given the formals `Xs` of the constructor of the class. The `findall` after that assembles the list of fields that, given the super invocation, should be inherited from the superclass. The append predicate makes sure that the fields assigned by the super invocation (`Gs`) and those declared by the class (`Fs`) are indeed those handled by the constructor, while invocation of `fields` on the superclass `D` makes sure that the parameters used in the super invocation correspond to the fields (inherited and declared) of `D`.

%CHECKME: see missing type check for S in the code above.

#### Program Typing

```prolog
ok(P) :-
    P = program(Cs),
    maplist(ok4all(P), Cs).
```

This rule is implicit in TAPL, Fig. 19-4.

### Summary

While the term typing rules are mostly straightforward to translate to Prolog, the necessary pattern matching in *Method typing* and *Class typing* reflects the distance from the chosen term encoding of the abstract syntax tree in Prolog to the use of (or reference to) concrete syntax present in the original rules. I am not sure how this could be improved by chosing a different AST encoding in Prolog, and what its consequences in other places are. @Ralf?

# ToDos

@{Ralf, Vadim}:

Please vote: Should I try to introduce operators like '|-' and use ':' to make the Prolog rules resemble the inference rules of TAPL more closely?

Please vote: Should I introduce additional auxiliary functions to implement metavariable binding in the inference rules (e.g., use something like value_of_field_i(...) rather than nth0(...), nth0(...) in E-ProjNew)?

Please vote: Should I try to rewrite type relations using operators '|-', ':', and '<:'?

@Ralf: Your experience with supporting type safety arguments by analyzing the above rules from within Prolog would be greatly appreciated!

@Vadim: can I have an overline, please?
