# Higher-order attribute

## Also known as
Non-terminal attribute (NTA)

## Definition
An attribute whose value is a fresh [AST](abstract_syntax_tree.md) subtree.

Allows new pieces of AST to be derived from the original AST. The node containing the higher-order attribute is regarded as the parent of the subtree. Is higher-order in that it can itself have attributes. In this case its inherited attributes should be defined by its parent node. Can be [synthesized](synthesized_attribute.md) or [inherited](inherited_attribute.md). 

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Harald Vogt, S. Doaitse Swierstra, Matthijs F. Kuiper: Higher-Order Attribute Grammars. PLDI 1989: 131-145. http://doi.acm.org/10.1145/73141.74830

## Example uses
* Macro-like expansion
* Reification of implicit structure 





