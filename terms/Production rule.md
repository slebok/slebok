# Production rule

## Definition
A rule describing which symbols may replace other symbols in a grammar. In a [Context-free grammar](Context-free grammar), the left-hand side consists of a single [Nonterminal symbol](Nonterminal symbol), while the right-hand side may be any sequence of [terminals](Terminal symbol) and nonterminals. For example, ```Expr ::= Expr "+" Expr``` says that anywhere you may have an expression, you can have an expression plus another expression. The rules may be used to generate syntactically correct strings, by applying them as rewrite rules starting with the [Start symbol](Start symbol), or be used to parse strings, e.g., in a [Top-down parser](Top-down parser) or [Bottom-up parser](Bottom-up parser).

## Tags
Syntax


