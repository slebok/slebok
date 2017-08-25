# Recursive attribute evaluation algorithm

## Definition
Evaluates an attribute by calling its semantic function, which may call other attributes' semantic functions recursively. Can memoize resulting attribute values to obtain optimal evaluation (evaluating each attribute at most once).

This algorithm is [demand-driven](demand-driven_attribute_evaluation.md) and can handle [reference](reference_attribute.md) and [parameterized](parameterized_attribute.md) attributes.

## Classification
[attribute grammar](attribute_grammar.md) \>  [attribute evaluation algorithm](attribute_evaluation_algorithm.md) \> (this term)

## First mention
* Martin Jourdan: An Optimal-time Recursive Evaluator for Attribute Grammars. Symposium on Programming 1984: 167-178 https://doi.org/10.1007/3-540-12925-1_37
