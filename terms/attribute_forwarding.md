# Attribute forwarding

## Definition
A technique allowing an access to an attribute to be delegated (*forwarded*) to a corresponding attribute in an [AST](abstract_syntax_tree.md) constructed by a [higher-order attribute](higher-order_attribute.md).

Forwarding supports [desugaring](desugaring.md) AST transformations in a modular way. When a language construct *New* is introduced, a higher-order attribute can be added in *New* that is defined as the desugared version of *New*, using existing language constructs, say *Old*. Through forwarding from *New* to *Old*, client accesses of attributes in *New* are by default delegated to corresponding attributes in *Old*. This allows reuse of existing attribute definitions in *Old*, for example for name analysis, type checking, code generation, etc. The forwarding can be overridden for individual attributes to define any behavior that is different between *New* and *Old*, for example for new kinds of static-semantic errors.

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Eric Van Wyk, Oege de Moor, Kevin Backhouse, Paul Kwiatkowski:
Forwarding in Attribute Grammars for Modular Language Design. CC 2002: 128-142
https://doi.org/10.1007/3-540-45937-5_11

## Example uses
* Defining the Java *enhanced-for* statement in terms of Java's original C-like *for* statement.




