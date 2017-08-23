# Associativity

## Definition
A property of binary operators in parsing, indicating whether expressions such as *a + b + c* should be interpreted as *(a + b) + b* (left associative), *a + (b + c)* (right associative) or as illegal (non-associative). 
#### Left
Operations are grouped on the left, giving a tree which is “heavy” on the left side; typically used for arithmetic operators. Without [associativity rules](Associativity rule), grammar productions usually look like ```PlusExpr ::= PlusExpr "+" MultExpr | ...```, forcing any expression containing the operator to be on the left side.
 
#### Right
Operations are grouped on the right, giving a tree which is “heavy” on the right side; typically used for assignment and exponentiation operators.


## Links


[[Wikipedia](http://en.wikipedia.org/wiki/Operator_associativity)]

## Tags
Ambiguity,Syntax


