# Circular attribute

## Definition
An attribute that is dependent on itself, leading to a cycle in the dependency graph. Evaluated by fixed-point iteration.

To be well-defined, there should be a bottom value defined for the attribute, and all semantic functions on the cycle should be monotonic, with values on a lattice of finite height.

Typically, set values are used with the empty set as the bottom value and union as the monotonic operator. If the lattice top is a finite set of elements, e.g., the set of all declarations in a given program, the lattice will be of finite height.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Rodney Farrow: Automatic generation of fixed-point-finding evaluators for circular, but well-defined, attribute grammars. SIGPLAN Symposium on Compiler Construction 1986: 85-98. http://doi.acm.org/10.1145/12276.13320

## Example uses
* Data-flow analysis
* Other static analyses, based on closures.
* Detection of cyclic class hierarchies 


