# Attribute evaluation algorithm

## Definition
An algorithm that automatically computes values for attributes in an [abstract syntax tree](abstract_syntax_tree.md),
according to their definitions in an attribute grammar.

Evaluation algorithms can be classified as static/dynamic.
*Static* algorithms use an evaluation order computed ahead of evaluation time,
based on the attribute dependencies in the attribute grammar,
approximating the dependencies between attribute instances in any [abstract syntax tree](abstract_syntax_tree.md).
A widely used static algorithm is Kastens' algorithm for *ordered attribute grammars*. It represents the precomputed evaluation
orders as, so called, *visit sequences*.
*Dynamic* algorithms use an evaluation order computed at evaluation time,
based on dependencies between attribute instances in a particular [abstract syntax tree](abstract_syntax_tree.md).
A widely used dynamic algorithm is Jourdan's algorithm based on recursion and memoization. 
This algorithm is more powerful than the static algorithms and can handle, e.g.,
[reference attributes](reference_attribute.md).

Evaluation algorithms can be exhaustive/demand-driven.
The static algorithms are typically *exhaustive* in that they evaluate all attributes in a tree.
The recursive dynamic algorithm is *demand-driven* in that it evaluates only the attributes a client requests the values for
(and any additional attributes needed to evaluate the requested attributes).

Evaluation algorithms can be batch/incremental. In *batch* evaluation, the abstract syntax tree is immutable,
and each attribute needs to be evaluated at most once. In *incremental* evaluation, the tree can be mutated
(for example by an editor), and any attributes dependent on the change are updated.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## Links
* Uwe Kastens: Ordered Attributed Grammars. Acta Inf. 13: 229-256 (1980) https://doi.org/10.1007/BF00288644
* Martin Jourdan: An Optimal-time Recursive Evaluator for Attribute Grammars. Symposium on Programming 1984: 167-178 https://doi.org/10.1007/3-540-12925-1_37
