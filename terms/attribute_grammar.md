# Attribute grammar (AG)

## Definition
An extension of [context-free grammars](context-free_grammar.md) with derived properties ("attributes") of the [nonterminals](nonterminal_symbol.md). The derived property values are defined using [semantic functions](semantic_function.md), associated with the [productions](production_rule.md). Can be used to automatically compute static properties of [abstract syntax tree](abstract_syntax_tree.md) nodes. Typical examples include name bindings, types of expressions, error messages, generated code, and other properties useful inside compilers. Other examples include name completion menus, cross references, and other properties useful in language-based editors. Classical attribute grammars (as introduced by Knuth) include two kinds of attributes: [synthesized](synthesized_attribute.md) and [inherited](inherited_attribute.md). Later generalizations include additional mechanisms like [higher-order attributes](higher-order_attribute.md), [circular attributes](circular_attribute.md), [collection attributes](collection_attribute.md), [reference attributes](reference_attribute.md), [parameterized attributes](parameterized_attribute.md), [rewrites](attribute_conditional_rewrite.md), and [forwarding](attribute_forwarding.md). Different [attribute evaluation algorithms](attribute_evaluation_algorithm.md) can be used to automatically compute the attribute values.

## First Mention
Donald E. Knuth, “Semantics of context-free languages,” Mathematical Systems Theory 2 (1968), 127–145. https://doi.org/10.1007/BF01692511

## Links
* this sameAs http://en.wikipedia.org/wiki/Attribute_grammar
* #Semantics
