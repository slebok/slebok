# Attribute grammar (AG)

## Definition
An extension of [context-free grammars](context-free_grammar.md) with derived properties ("attributes") of the [nonterminals](nonterminal_symbol.md). The derived property values are defined using directed equations known as [semantic functions](semantic_function.md), associated with the [productions](production_rule.md).

Can be used to automatically compute static properties of [abstract syntax tree](abstract_syntax_tree.md) nodes. Typical examples include name bindings, types of expressions, error messages, generated code, and other properties useful inside compilers. Other examples include name completion menus, cross references, and other properties useful in language-based editors. [Classic attribute grammars](classic_attribute_grammar.md) support tree-based computations. [Reference attribute grammars)(reference_attribute_grammar.md) additionally support graph-based computations.

Attribute kinds include [synthesized](synthesized_attribute.md), [inherited](inherited_attribute.md), [higher-order](higher-order_attribute.md), [circular](circular_attribute.md), [collection](collection_attribute.md), [reference](reference_attribute.md), and [parameterized](parameterized_attribute.md) attributes. Additional mechanisms include [forwarding](attribute_forwarding.md) and [attribute-conditional rewrites](attribute_conditional_rewrite.md).

Different [attribute evaluation algorithms](attribute_evaluation_algorithm.md) can be used to automatically compute the attribute values.

## First Mention
Donald E. Knuth, “Semantics of context-free languages,” Mathematical Systems Theory 2 (1968), 127–145. https://doi.org/10.1007/BF01692511

## Links
* this sameAs http://en.wikipedia.org/wiki/Attribute_grammar
* #Semantics
