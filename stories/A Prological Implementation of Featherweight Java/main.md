# A Prological Treatise of Featherweight Java

This story tells how I reconstructed parser, evaluator and type checker of Featherweight Java (FJ) as described in TAPL, Sect. 19, in Prolog (SWI-Prolog, to be precise). The insights of this story, if any, come from observing
1. how the implementation reconstructs the semantics of the book's figures' specifying syntax and semantics of FJ, and 
1. how the implementation deviates from the specifications (and why).

My work was partly motivated by Guy Steele's talk ["It’s Time for a New Old Language"](https://www.youtube.com/watch?v=dCuZkaaou0Q).

## Syntax and Parser

The syntax of FJ is given in TAPL, Fig. 19-1:

(include figure here)

Note that the grammar uses nonterminals and metavariables (the latter are introduced in the book's accompanying text, and here can be identified by the absence of corresponding productions).

There are four things I'd like to point out:
1. The overline notation replaces for the Kleene star found in other grammar languages, subject to conventions that, had they not been provided in the accompanying text, could have only be derived from another syntax specification of Java. The grammar as is can only be interpreted using extra knowledge; it cannot be used to drive a parser.
1. The grammar for terms is left recursive; also, the left associativity of member access requires some special treatment.
1. The grammar for terms does not introduce parantheses, even though these are required for member access on cast expressions.
1. Fig. 19-1 really specifies two grammars, one for programs (including terms) and one for values. The language of values is a sublanguage of the language of terms (all values are terms syntactically).

The first finding is justified by the fact that the bindings of the metavariables (as performed by the parser) immediately serve the evaluation and typying rules.

The grammar is reconstructed as a DCG in Prolog as follows:

```prolog
'P'(program(P)) --> repeating('CL'(P)).
```

This rule (the nonterminal `'P'`) is implicit in TAPL. The (non-ground) term `program(P)` serves the construction of the syntax tree; the root node is a term of type `program`. (Unfortunately, SWI-Prolog has no way of specifying types.) `repeating` is a meta-predicate that implements the Kleene star for its argument, a nonterminal. In the above rule, it effects to P being unified with a list of nodes representing class definitions (possibly empty).
