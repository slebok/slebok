# Reference attribute

## Definition
An attribute whose value is a reference to a node in an [AST](abstract_syntax_tree.md). [Synthesized](synthesized_attribute.md) and [inherited](inherited_attribute.md) reference attributes can be used to derive graph edges that extend the AST to a graph.

[Semantic functions](semantic_function.md) are allowed to access attributes via reference attributes, using qualified access, thereby allowing new attribute values to be computed using the derived graph.

By using [intrinsic](intrinsic_attribute.md) reference attributes, an [abstract syntax graph](abstract_syntax_graph.md) can be expressed as an AST spanning tree with graph edges. This allows [reference attribute grammars](reference_attribute_grammar.md) to be used directly for abstract syntax graphs, for example, on an ECore [metamodel](metamodel.md).

## Classification
[attribute grammar](attribute_grammar.md) \> [reference attribute grammar](reference_attribute_grammar.md) \> (this term)

## Example uses
* Name bindings
* Inheritance graph
* Call graph




