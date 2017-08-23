# Algebraic data type

## Definition
A composite data type defined inductively from other types. Typically, each type has a number of cases or alternatives, which each case having a *constructor* with zero or more arguments.

For example, ```data Expr = Int(int i) | Plus(Expr a, Expr b)```. The data type can be seen as an algebraic *signature*, with the expressions written using the constructors being *terms*. For our purposes, values may be interpreted as trees, with the constructor name being node labels, arguments being children, and atomic values and nullary constructors being leaves.

## Meta
* this sameAs http://en.wikipedia.org/wiki/Algebraic_data_type
* this sameAs http://tutor.rascal-mpl.org/Rascal/Declarations/AlgebraicDataType/AlgebraicDataType.html
* #Semantics
* #Types
