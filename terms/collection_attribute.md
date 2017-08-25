# Collection attributes

## Definition
An attribute that is defined by a combination of a set of "contributions" associated with nonterminals in the [AST](abstract_syntax_tree.md), i.e., a fold over information in the complete AST.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
John Tang Boyland. Descriptional Composition of Compiler Components. PhD thesis, University of California, Berkeley, 1996. Available as technical report UCB//CSD-96-916.

## Example uses
* Collection of compile-time error messages
* Computation of reverse references. For example, computing the set of uses of a given declaration, given that each use has a binding reference to its declaration.
