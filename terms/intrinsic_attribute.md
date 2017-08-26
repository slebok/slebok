# Intrinsic attribute

## Definition
A property of an [AST](abstract_syntax_tree.md) node that gets its value as part of the construction of the node (for example, during parsing), rather than being derived using a rule like other [attributes](attribute_grammar.md). A typical example is a token.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Rodney Farrow:
LINGUIST-86: Yet Another Translator Writing System Based On Attribute Grammars. SIGPLAN Symposium on Compiler Construction 1982: 160-171.  http://doi.acm.org/10.1145/800230.806992

## Example uses
* Intrinsic node properties like tokens
* Abstract syntax graph (extending an AST spanning tree to a graph, using intrinsic [reference attributes](reference_attribute.md))
* (Åkesson 2010) used intrinsic [reference attributes](reference_attribute.md) to let a Modelica model instance reference its model class.

## Links
* Johan Åkesson, Torbjörn Ekman, Görel Hedin:
Implementation of a Modelica compiler using JastAdd attribute grammars. Sci. Comput. Program. 75(1-2): 21-38 (2010). https://doi.org/10.1016/j.scico.2009.07.003
