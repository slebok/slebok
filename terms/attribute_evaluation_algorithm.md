# Attribute evaluation algorithm

## Definition
An algorithm that automatically computes values for attributes in an [abstract syntax tree](abstract_syntax_tree.md),
according to their definitions in an attribute grammar.

Evaluation algorithms can be classified as
* [static](static_attribute_evaluation.md) or [dynamic](dynamic_attribute_evaluation.md) depending on when the attribute evaluation order is computed.
* [exhaustive](exhaustive_attribute_evaluation.md) or [demand-driven](demand-driven_attribute_evaluation) depending on if all attributes are evaluated, or only those requested by a client.
* [incremental](incremental_attribute_evaluation.md) or [batch](batch_attribute_evaluation.md) depending on if the abstract syntax tree is mutable or immutable.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)
