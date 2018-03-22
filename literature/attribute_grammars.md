# Attribute grammars

Knuth first described attribute grammars in his 1968 paper “Semantics of context-free languages”.
The key insight was to augment a [context-free grammars](../terms/context-free_grammar.md) with equations that describe how to compute attribute values associated with grammar symbols.
A simple evaluation approach enables attribute values to be computed for arbitrary trees that conform to the grammar.

* Donald E. Knuth. Semantics of context-free languages. Mathematical Systems Theory 2 (1968), 127–145. [DOI](https://doi.org/10.1007/BF01692511)

Knuth presented a retrospective on the early development of attribute grammars in a 1990 invited conference paper.

* Donald E. Knuth, "The Genesis of Attribute Grammars", Proceedings of the International Conference on Attribute Grammars and their Applications (1990), 1-12. [DOI](https://doi.org/10.1007/3-540-53101-7_1)

Knuth's formulation of attribute grammars inspired many investigations in the 1970-90s.
The motivations behind attribute grammars and developments in this period were surveyed by Paakki.

* Jukka Paakki. Attribute grammar paradigms: a high-level methodology in language implementation. ACM Computing Surveys 27:2 (June 1995), 196–255. [DOI](https://doi.org/10.1145/210376.197409)

Modern attribute grammar systems go well beyond Knuth's original ideas, including to references so that graph-structures can be computed and attributed, attributes that iteratively evaluate to a fixed-point, and notations for defining attribute grammars in a modular fashion.

## Applications

Attribute grammars were originally developed for static analysis of programming languages.
For example, attributes can encode naming or typing information and can be used to enforce conditions on the programs that a compiler accepts.

FIXME: more

However, an attribute grammar can be used for any problem that can be formulated in terms of tree decoration.

FIXME: application areas, success stories

## Attribute kinds

FIXME: [synthesized](../terms/synthesized_attribute.md), [inherited](../terms/inherited_attribute.md), [intrinsic](../terms/intrinsic_attribute.md)

FIXME: [higher-order](../terms/higher-order_attribute.md) Vogt, [forwarding](../terms/attribute_forwarding.md) Van Wyk

FIXME: [reference](../terms/reference_attribute.md) Hedin, [parameterized](../terms/parameterized_attribute.md)

FIXME: [circular](../terms/circular_attribute.md) Farrow

## Modularity

FIXME: [copy rules](../terms/attribute_copy_rule.md)

FIXME: symbol computations, abstract roles

FIXME: remote attributes

FIXME: [collection](../terms/collection_attribute.md) Boyland thesis

FIXME: OO AGs (Koskimies)

FIXME: aspects

## Evaluation

A key focus of research on attribute grammars is on efficient [attribute evaluation](../terms/attribute_evaluation_algorithm).
The need is to obtain a schedule that determines the order in which attribute occurrences in a structure (tree or graph) will be evaluated.
A valid schedule must obey the dependencies between attribute occurrences as expressed by the equations.

It may be possible to [statically analyse attribute equations](../terms/static_attribute_evaluation.md) to determine a schedule.
A notable example of this approach is Kastens' class of *Ordered Attribute Grammars* for which a single schedule can be determined that will work for any conforming syntax tree.

* Uwe Kastens. Ordered Attribute Grammars. Acta Informatica, 13:3 (March 1980), 229-256. [DOI](https://doi.org/10.1007/BF00288644)

Static analysis approaches require that the attribute grammar is well-defined.
For example, circular equations where an attribute value depends on itself would be outlawed.
Many researchers have developed methods for detecting circularity.

FIXME: ref, feasibility, complexity?

Others developed [circular attributes](../terms/circular_attribute.md) for use in cases where the computations can be evaluated to a fixed point.

In contrast to static approaches, a schedule may be [computed dynamically](../terms/dynamic_attribute_evaluation.md).
Notably, Jourdan showed how any attribute grammar can be evaluated by a collection of recursive functions.
A downside of dynamic approaches is that properties such as circularity can only be checked during evaluation.

* Martin Jourdan. 1984. An optimal-time recursive evaluator for attribute grammars. In Proceedings of the International Symposium on Programming (Lecture Notes in Computer Science), Vol. 167. 167–178. [DOI](https://doi.org/10.1007/3-540-12925-1_37)

Later, Johnson showed how non-circular attribute grammars can be naturally evaluated as lazy functional programs.
In this approach the scheduling is performed by the lazy evaluation mechanism.

* Thomas Johnsson. Attribute grammars as a functional programming paradigm. Conference on Functional Programming Languages and Computer Architecture (1987), 154-173. [DOI](https://doi.org/10.1007/3-540-18317-5_10)

## Evaluation modes

FIXME: evaluation as tree is constructed, S-attributed etc

[incremental](../terms/incremental_attribute_evaluation.md) or [batch](../terms/batch_attribute_evaluation.md)

[exhaustive](../terms/exhaustive_attribute_evaluation.md) or [demand-driven](../terms/demand-driven_attribute_evaluation.md)

Instead of evaluating all of the attributes in one go, an incremental approach can be used to only evaluate attributes that are needed.
For example, in an interactive editing environment the tree might represent a program that is being edited and attributes will be evaluated incrementally in response to user edit actions.

A notable early example of this application is Reps' Synthesizer Generator that used incremental techniques to evaluate attributes on a tree that is being edited by the user of a structure editor.

## Software

Many attribute-grammar based systems, tools and libraries have been developed.
The following list includes particularly notable examples, but does not aim to be complete.

Eli, GAG, [JastAdd](../tools/JastAdd.md), [Kiama](../tools/Kiama.md), LIGA, LISA, LRC, [Silver](../tools/Silver.md)

FIXME: more needed here

## TODO

FIXME: [attribute-dependent rewrites](../terms/attribute_dependent_rewrite.md)

[Classic attribute grammars](../terms/classic_attribute_grammar.md) vs [Reference attribute grammars](../terms/reference_attribute_grammar.md)

directed equations known as [semantic functions](../terms/semantic_function.md)
