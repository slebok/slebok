# Dynamic attribute evaluation algorithm

## Definition
Uses an evaluation order computed at attribute evaluation time,
based on dependencies between attribute instances in a particular [abstract syntax tree](abstract_syntax_tree.md).

Is in general more powerful than a [static evaluation algorithm](static_attribute_evaluation.md), but does not detect unintentional circular attribute dependencies until attribute evaluation time.

A widely used dynamic algorithm is Jourdan's [recursive algorithm](recursive_attribute_evaluation.md).

## Classification
[attribute grammar](attribute_grammar.md) \>  [attribute evaluation algorithm](attribute_evaluation_algorithm.md) \> (this term)
