# Predictive parser

## Definition
A [Recursive descent parser](Recursive descent parser) which does not require backtracking. Instead, it looks ahead a finite number of tokens and decides which parsing function should be called next. The grammar must be LL(*k*) for this to work, where *k* is the maximum lookahead.

## Tags
Parsing


