# Attribute grammars

Knuth first described *attribute grammars* in his 1968 paper "Semantics of context-free languages".
The key insight was to augment a context-free grammar with equations that describe how to compute attribute values associated with grammar symbols.
A simple evaluation approach enables attribute values to be computed for arbitrary trees that conform to the grammar.

Donald E. Knuth. Semantics of context-free languages. Mathematical Systems Theory 2 (1968), 127–145. https://doi.org/10.1007/BF01692511

Knuth presented a retrospective on the early development of attribute grammars in a 1990 invited conference paper.

Donald E. Knuth, "The Genesis of Attribute Grammars", Proceedings of the International Conference on Attribute Grammars and their Applications (1990), 1-12. https://doi.org/10.1007/3-540-53101-7_1

Knuth's formulation of attribute grammars inspired many investigations in the 1970-90s.
The motivations behind attribute grammars and developments up to the mid-1990s were surveyed by Paakki.

Jukka Paakki. Attribute grammar paradigms: a high-level methodology in language implementation. ACM Computing Surveys 27:2 (June 1995), 196–255. https://doi.org/10.1145/210376.197409

FIXME: this gap illustrates a need for a more up to date survey which would be referenced here

## Applications

Attribute grammars were originally developed for static analysis of programming languages
For example, attributes can encode naming or typing information and can be used to enforce conditions on the programs that a compiler accepts.

However, an attribute grammar can be used for any problem that can be formulated in terms of tree decoration.

FIXME: list some more application areas, give some success stories

## Static versus dynamic evaluation algorithms

*Static* algorithms use an evaluation order computed ahead of evaluation time,
based on the attribute dependencies in the attribute grammar,
approximating the dependencies between attribute instances in any [abstract syntax tree](abstract_syntax_tree.md).
A widely used static algorithm is Kastens' algorithm for *ordered attribute grammars*. It represents the precomputed evaluation
orders as, so called, *visit sequences*.

Uwe Kastens. Ordered Attribute Grammars. Acta Informatica, 13:3 (March 1980), 229-256. https://doi.org/10.1007/BF00288644

Static evaluation often requires that the attribute grammar has certain properties.
For example, circular definitions where an attribute value depends on itself would be outlawed.
Many researchers have developed methods for detecting circularity.

FIXME: ref, feasibility, complexity?

*Dynamic* algorithms use an evaluation order computed at evaluation time,
based on dependencies between attribute instances in a particular [abstract syntax tree](abstract_syntax_tree.md).
A widely used dynamic algorithm is Jourdan's that is based on recursion and memoization. 
This algorithm is more powerful than the static algorithms and can handle, e.g.,
[reference attributes](reference_attribute.md).

Martin Jourdan. 1984. An optimal-time recursive evaluator for attribute grammars. In Proceedings of the International Symposium on Programming (Lecture Notes in Computer Science), Vol. 167. 167–178. https://doi.org/10.1007/3-540-12925-1_37

Later, Johnson showed how non-circular attribute grammars can be naturally evaluated as lazy functional programs.
In this approach the scheduling is performed by the lazy evaluation mechanism.

Thomas Johnsson. Attribute grammars as a functional programming paradigm. Conference on Functional Programming Languages and Computer Architecture (1987), 154-173. https://doi.org/10.1007/3-540-18317-5_10

## Extensions

Many researchers have developed extensions of basic Knuth-style attribute grammars.

OO AGs (Koskimies)

higher-order attributes (Vogt)
- forwarding (Van Wyk)

reference attributes (Hedin)
- parameterised attributes

circular attributes (Farrow)

collection attributes (Boyland thesis)

modular techniques
- symbol computations, abstract roles, GAG, LIGA (Kastens)
- aspects, JastAdd (Hedin)
- something by Farrow

## Incremental evaluation

Instead of evaluating all of the attributes in one go, an incremental approach can be used to only evaluate attributes that are needed.
For example, in an interactive editing environment the tree might represent a program that is being edited and attributes will be evaluated incrementally in response to user edit actions.

A notable early example of this application is Reps' Synthesizer Generator that used incremental techniques to evaluate attributes on a tree that is being edited by the user of a structure editor.

FIXME: ref

FIXME: more modern usage?

## Tools

FIXME: JastAdd, Silver, Kiama (functional), LRC, Haskell-based (LRC, Swierstra), (Aspect)LISA

## TODO

FIXME: parser-based evaluation, ANTLR

FIXME: attribution of non-trees

