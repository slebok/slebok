# LL parser

## Definition
A table-driven [Top-down parser](Top-down parser), similar to a [Recursive descent parser](Recursive descent parser). Has trouble dealing with [Left recursion](Left recursion) in production rules, so the grammar must typically be [left factored](Left factoring) prior to use. The LL parser reads its input in one direction (left-to-right) and produces a leftmost [Derivation](Derivation), hence the name LL. Often referred to as LL(*k*), where the *k* indicates the number of tokens of lookahead the parser uses to avoid backtracking.

## Links


[[Wikipedia](http://en.wikipedia.org/wiki/LL_parser)]

## Tags
Parsing


