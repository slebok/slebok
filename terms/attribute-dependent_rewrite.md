# Attribute-dependent rewrite

## Definition
A technique allowing an original [AST](abstract_syntax_tree.md) to be automatically transformed into a derived AST, using rewrite rules that use attributes to define the transformation.

The rewrite rules are applied using a fixed-point algorithm, until all rewrite rules are fulfilled. In the original formulation by Ekman, the AST was mutated. It was later discovered by Söderberg that attribute-dependent rewrites could be implemented using [circular](circular_attribute.md) [higher-order](higher-order_attribute.md) attributes. This allows the derived AST to be computed without mutating the original AST, which is a prerequisite for [incremental](incremental_attribute_evaluation.md) and [concurrent](concurrent_attribute_evaluation.md) attribute evaluation.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Torbjörn Ekman, Görel Hedin: Rewritable Reference Attributed Grammars. ECOOP 2004: 144-169. https://doi.org/10.1007/978-3-540-24851-4_7

## Links
* Emma Söderberg, Görel Hedin: Declarative rewriting through circular nonterminal attributes. Computer Languages, Systems & Structures 44: 3-23 (2015). https://doi.org/10.1016/j.cl.2015.08.008

## Example uses
* Desugaring
* Specializing identifier nodes depending on their types.




