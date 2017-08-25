# Dynamic attribute evaluation

## Definition
Uses an evaluation order computed at attribute evaluation time,
based on dependencies between attribute instances in a particular [abstract syntax tree](abstract_syntax_tree.md).

A widely used dynamic algorithm is Jourdan's algorithm based on recursion and memoization. 
This algorithm is [demand-driven](demand-driven_attribute_evaluation.md) and more powerful than [static attribute evaluation](static_attribute_evaluation.md). It can handle [reference attributes](reference_attribute.md).

## Classification
[attribute grammar](attribute_grammar.md) \>  [attribute evaluation algorithm](attribute_evaluation_algorithm.md) \> (this term)

## Links
* Martin Jourdan: An Optimal-time Recursive Evaluator for Attribute Grammars. Symposium on Programming 1984: 167-178 https://doi.org/10.1007/3-540-12925-1_37
