# Abstract syntax tree (AST)

## Definition
A tree representation of the syntactic structure of a sentence; similar to a parse tree, but usually ignoring literal nonterminals and nodes corresponding to production rules that don't directly contribute to the structure of the language (e.g., parenteses, productions used to encode [operator priority](priority rule) and so on). May represent a slightly simpler language that the parse tree, for example, with operator calls [desugared](desugaring) to function calls, and variations of a construct folded into one. Can be represented using as trees or terms, and described by an algebraic data type or a regular tree grammar. 

## Links

* this sameAs http://en.wikipedia.org/wiki/Abstract_syntax_tree
* #Abstraction
* #Parsing
* #Syntax
