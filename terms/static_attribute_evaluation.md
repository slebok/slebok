# Static attribute evaluation algorithm
## Definition
Uses an evaluation order computed ahead of evaluation time, based on the dependencies in the attribute grammar, approximating the dependencies between attribute instances in any [abstract syntax tree](abstract_syntax_tree.md). Is typically [exhaustive](exhaustive_attribute_evaluation.md).

A widely used static algorithm is Kastens' algorithm for *ordered attribute grammars*. It represents the precomputed evaluation
orders as, so called, *visit sequences*.

## Classification
[attribute grammar](attribute_grammar.md) \> [attribute evaluation algorithm](attribute_evaluation_algorithm.md) \> (this term)

## Links
* Uwe Kastens: Ordered Attributed Grammars. Acta Inf. 13: 229-256 (1980) https://doi.org/10.1007/BF00288644
