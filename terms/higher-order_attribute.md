# Higher-order attribute

## Also known as
Nonterminal attribute (NTA)

## Definition
An attribute whose value is a fresh [AST](abstract_syntax_tree.md) subtree.

Allows new pieces of AST to be derived from the original AST, and extending it. The attribute is higher-order in that the new subtree can itself have attributes. In this case the inherited attributes of the subtree root should be defined by the node (nonterminal) declaring the higher-order attribute. This node acts as the parent of the new subtree.

The commonly used synonym *nonterminal attribute* indicates that the higher-order attribute is both an attribute and a nonterminal.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Harald Vogt, S. Doaitse Swierstra, Matthijs F. Kuiper: Higher-Order Attribute Grammars. PLDI 1989: 131-145. http://doi.acm.org/10.1145/73141.74830

## Example uses
* Macro-like expansion
* Reification of implicit structure 





