# LL parser

## Definition
A table-driven [top-down parser](top-down_parser.md), similar to a [recursive descent parser](recursive_descent_parser.md). Has trouble dealing with left recursion in production rules, so the grammar must typically be left factored prior to use. The LL parser reads its input in one direction (left-to-right) and produces a leftmost derivation, hence the name LL. Often referred to as LL(*k*), where the *k* indicates the number of tokens of lookahead the parser uses to avoid backtracking.

## Meta
* this sameAs http://en.wikipedia.org/wiki/LL_parser
* #Parsing
