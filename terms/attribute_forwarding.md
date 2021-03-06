# Attribute forwarding

## Definition
A technique allowing an access to an attribute to be delegated (*forwarded*) to a another "semantically equivalent" [AST](abstract_syntax_tree.md) that is constructed as a designated [higher-order attribute](higher-order_attribute.md).

Forwarding supports [desugaring](desugaring.md) AST transformations in a modular way. When a language construct *New* is introduced, a higher-order attribute can be added in *New* that is defined as the desugared version of *New*, using existing language constructs, say *Old*. Through forwarding from *New* to *Old*, client accesses of attributes without a defining equation on *New* are delegated to corresponding attributes in *Old*. This allows reuse of existing attribute definitions in *Old*, for example for code generation. Explicit equations for other attributes are allowed and these take precedence over the values on the forwarded-to tree, that is on *Old*.  This is useful to define attributes for, for example, type checking and error messages, so that static errors can given using the code the programmer wrote (*New*) and not its desugaring (*Old*).

## Classification
[attribute grammar](attribute_grammar.md) \> (this term)

## First mention
Forwarding was part of the Intentional Programming project at Microsoft Research.  It was incorported into attribute grammars in 2002.

Eric Van Wyk, Oege de Moor, Kevin Backhouse, Paul Kwiatkowski:
Forwarding in Attribute Grammars for Modular Language Design. CC 2002: 128-142
https://doi.org/10.1007/3-540-45937-5_11

## Example use

Forwarding supports the modular specification of language extensions. One extension can add a new construct that forwards to old constructs in a host language.  A second extension can add a new semantic analysis as attributes and equations defined over the host language grammar.  These independently-developed extensions then work together as the new analysis, when applied to the new constructs in the first extension, are carried on their translation to the host language.

Defining the Java *enhanced-for* statement in terms of Java's original C-like *for* statement.  Attributes defining static errors can be computed on the original *enhanced-for* production so that error messages are in terms of the code written by the programmer. But an attribute defining its translation to Java bytecode can be computed via forwarding to the translation using an original *for* statement since they are semantically the same.

Another extension doing a static analysis of Java code can be implemented separately as equations and attributes over the Java host grammar. The two extensions can then be composed without any additional specifications needing to be written.



