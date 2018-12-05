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
1. The grammar for terms does not introduce parantheses, even though these are required for member access on cast expressions.
1. Fig. 19-1 really specifies two grammars, one for programs (including terms) and one for values. The language of values is a sublanguage of the language of terms (all values are terms syntactically).

All findings are justified by the primary use of the grammar: providing an inductive definition of the language (or, rather, its syntax trees) suited to serve the evaluation and typing rules. 

### Syntax and Parser in Prolog

A grammar that is also suitable for parsing is reconstructed as a Definite Clause Grammar (DCG) in Prolog as follows:

```prolog
'P'(program(P)) --> repeating('CL'(P)).
```

This (start) rule is implicit in TAPL. The (non-ground) term `program(P)` that is an argument to the start nonterminal `'P'` (quoted becaue in Prolog, unquoted identifiers starting with a capital letter are interpreted as variables) serves the construction of the syntax tree; the (root) node is a term of type `program`. (Unfortunately, Prolog is untyped and SWI-Prolog has no way of declaring structs.) `repeating` is a meta-predicate that implements the Kleene star for its argument, a nonterminal. In the above rule, it effects to P being unified with a list of nodes representing class definitions (possibly empty).

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

