# Abstract syntax tree (AST)

## Definition
> a tree representation of the deep syntactic structure of a sentence (cleaned from surface clutter needed only for parsing)

An abstract syntax tree is similar to a parse tree, but usually ignoring literal nonterminals and nodes corresponding to production rules that don't directly contribute to the structure of the language (e.g., parentheses, productions used to encode [operator priority](priority rule.md) and so on). May represent a slightly simpler language that the parse tree, for example, with operator calls [desugared](desugaring.md) to function calls, and variations of a construct folded into one. Can be represented using trees or terms, and described by an algebraic data type or a regular tree grammar. 

## Classification
Program Representation \> Abstract syntax tree

## Links

* this sameAs http://en.wikipedia.org/wiki/Abstract_syntax_tree
* this relatedTo [abstract syntax graph](abstract_syntax_graph.md)
* #Abstraction
* #Parsing
* #Syntax
