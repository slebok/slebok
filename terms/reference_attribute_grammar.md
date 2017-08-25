# Reference attribute grammar (RAG)

## Definition
An extension of [classic attribute grammars](classic_attribute_grammars.md) that supports [reference attributes](reference_attribute.md) which extend [abstract syntax trees](abstract_syntax_tree.md) to graphs.

Cannot use classic [static attribute evaluation](static_attribute_evaluation.md) since attribute dependencies may flow along the computed reference attribute edges. Is typically evaluated using [recursive attribute evaluation](recursive_attribute_evaluation.md). Detecting unintentional cyclic dependencies is undecidable in attribute grammars with dependencies along remote edges, and hence has to be done at attribute evaluation time (see Boyland 2005).

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Görel Hedin: Reference Attributed Grammars. Informatica (Slovenia) 24(3) (2000)

## Links
* John Tang Boyland: Remote attribute grammars. J. ACM 52(4): 627-687 (2005) http://doi.acm.org/10.1145/1082036.1082042

## Known Tools
* [JastAdd](http://jastadd.org)
* Silver
* Kiama
* JavaRAG
* RACR

