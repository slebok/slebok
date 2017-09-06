# Attribute grammars

Knuth first described attribute grammars in his 1968 paper “Semantics of context-free languages”.
The key insight was to augment a context-free grammar with equations that describe how to compute attribute values associated with grammar symbols.
A simple evaluation approach enables attribute values to be computed for arbitrary trees that conform to the grammar.
Knuth presented a retrospective on the early development of attribute grammars in a 1990 invited conference paper.

* Donald E. Knuth. Semantics of context-free languages. Mathematical Systems Theory 2 (1968), 127–145. https://doi.org/10.1007/BF01692511
* Donald E. Knuth, "The Genesis of Attribute Grammars", Proceedings of the International Conference on Attribute Grammars and their Applications (1990), 1-12. https://doi.org/10.1007/3-540-53101-7_1

Knuth's formulation of attribute grammars inspired many investigations in the 1970-90s.
The motivations behind attribute grammars and developments in this period were surveyed by Paaki.

* Jukka Paakki. Attribute grammar paradigms: a high-level methodology in language implementation. ACM Computing Surveys 27:2 (June 1995), 196–255. https://doi.org/10.1145/210376.197409

FIXME: this gap illustrates a need for a more up to date survey which would be referenced here

## Applications

Attribute grammars were originally developed for static analysis of programming languages
For example, attributes can encode naming or typing information and can be used to enforce conditions on the programs that a compiler accepts.

However, an attribute grammar can be used for any problem that can be formulated in terms of tree decoration.

FIXME: list some more application areas, give some success stories

## Static versus dynamic scheduling

A key focus of research on attribute grammars is on efficient attribute evaluation.
The main need is to compute a schedule that determines the order in which attribute will be evaluated.
A valid schedule must obey the dependencies between attributes as expressed by the equations.

It may be possible to statically analyse attribute equations to determine a schedule.
A notable example of this approach is Kastens' class of ordered attribute grammars for which a single schedule can be determined that that will work for any conforming syntax tree. 

* Uwe Kastens. Ordered Attribute Grammars. Acta Informatica, 13:3 (March 1980), 229-256. https://doi.org/10.1007/BF00288644

Static analysis approaches require that the attribute grammar is well-defined.
For example, circular definitions where an attribute value depends on itself would be outlawed.
Many researchers have developed methods for detecting circularity.

FIXME: ref, feasibility, complexity?

In contrast to static approaches, the schedule may be computed dynamically.
Notably, Jourdan showed how any attribute grammar can be evaluated by a collection of recursive functions.
A downside of dynamic approaches is that properties such as circular definitions can only be detected during evaluation.

* Martin Jourdan. 1984. An optimal-time recursive evaluator for attribute grammars. In Proceedings of the International Symposium on Programming (Lecture Notes in Computer Science), Vol. 167. 167–178. https://doi.org/10.1007/3-540-12925-1_37

Later, Johnson showed how non-circular attribute grammars can be naturally evaluated as lazy functional programs.
In this approach the scheduling is performed by the lazy evaluation mechanism.

* Thomas Johnsson. Attribute grammars as a functional programming paradigm. Conference on Functional Programming Languages and Computer Architecture (1987), 154-173. https://doi.org/10.1007/3-540-18317-5_10

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

