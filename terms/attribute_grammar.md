# Attribute grammar (AG)

## Definition
An extension of [context-free grammars](context-free_grammar.md) with derived properties ("attributes") of the [nonterminals](nonterminal_symbol.md). The derived property values are defined using directed equations known as [semantic functions](semantic_function.md), associated with the [productions](production_rule.md).

Can be used to automatically compute static properties of [AST](abstract_syntax_tree.md) nodes. [Classic attribute grammars](classic_attribute_grammar.md) support tree-based computations. [Reference attribute grammars](reference_attribute_grammar.md) additionally support graph-based computations.

Attribute kinds include [synthesized](synthesized_attribute.md), [inherited](inherited_attribute.md), [intrinsic](intrinsic_attribute.md), [higher-order](higher-order_attribute.md), [circular](circular_attribute.md), [collection](collection_attribute.md), [reference](reference_attribute.md), and [parameterized](parameterized_attribute.md) attributes. Most attribute grammar systems support implicit [copy rules](attribute_copy_rule.md). Additional mechanisms include [forwarding](attribute_forwarding.md) and [attribute-dependent rewrites](attribute-dependent_rewrite.md) which both support [AST](abstract_syntax_tree.md) transformations to a certain extent.

Different [attribute evaluation algorithms](attribute_evaluation_algorithm.md) can be used to automatically compute the attribute values.

## First Mention
Donald E. Knuth, “Semantics of context-free languages,” Mathematical Systems Theory 2 (1968), 127–145. https://doi.org/10.1007/BF01692511

## Links
* this sameAs http://en.wikipedia.org/wiki/Attribute_grammar
* #Semantics

## Example uses
* Analysis in compilers (name analysis, type checking, compile-time error checking, preparation for code generation, ...)
* Static program analysis (in particular when circular attributes are supported)
* Analysis in language-based editors (code completion menus, declaration cross references, ...)
