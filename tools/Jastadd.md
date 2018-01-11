# JastAdd

## Links
- http://jastadd.org

## Description

JastAdd is a meta-compilation system that supports [Reference Attribute Grammars](../terms/reference_attribute_grammar.md) (RAGs). It is designed to support extensible implementation of compilers and other program analysis tools.

Some key features of JastAdd:

* Object-oriented abstract syntax
* Static aspects for modular specification of abstract syntax and attributes
* Reference attributes
* Parameterized attributes
* Higher-order attributes
* Collection attributes
* Circular attributes
* Attribute-dependent rewrites

JastAdd generates Java code, and can be used together with any Java-based parser generator that supports user-defined semantic actions.


## Examples
Here is an excerpt from the JastAdd PicoJava example - an implementation of a front end for a tiny object-oriented language. For the full example, see http://jastadd.org/web/examples.php?example=PicoJava.

```
// Abstract grammar:
...
Block ::= BlockStmt*;
ClassDecl: TypeDecl ::= [Superclass:IdUse] Body:Block;
VarDecl: Decl ::= Type:Access;
AssignStmt: Stmt ::= Variable:Access Value:Exp;
WhileStmt: Stmt ::= Condition:Exp Body:Stmt;
abstract Exp;
abstract Access:Exp;
abstract IdUse: Access ::= <Name:String>;
Dot:Access ::= ObjectReference:Access IdUse;
...

// Name resolution:
import java.util.HashSet;
aspect NameResolution {
  syn Decl Access.decl();
  eq IdUse.decl() = lookup(getName());
  eq Dot.decl() = getIdUse().decl();
  eq Block.getBlockStmt(int index).lookup(String name) {
    if (!localLookup(name).isUnknown())
      return localLookup(name);
    return lookup(name);
  }
  eq Dot.getIdUse().lookup(String name) =
    getObjectReference().decl().type().remoteLookup(name);
  ...
}
```

## Applications

Examples of mature applications of JastAdd:

1. [ExtendJ](http://extendj.org),
   an open source extensible Java compiler with a modular extensible architecture, supporting Java 1.4, 5, 6, 7, and 8. Has many extensions by both the JastAdd team and external users.

2. [JModelica.org](http://www.modelon.com/products/jmodelicaorg/), an open source Modelica platform for simulation and optimization of dynamic models. JModelica.org is supported by the company Modelon.


## Key sources

- T. Ekman, G. Hedin. The JastAdd system - modular extensible compiler construction. Science of Computer Programming 69(1-3): 14-26, Elsevier, 2007. [DOI](http://dx.doi.org/10.1016/j.scico.2007.02.003)
- T. Ekman, G. Hedin: The JastAdd extensible Java compiler. OOPSLA 2007: 1-18, ACM, 2007. [DOI](http://doi.acm.org/10.1145/1297027.1297029)
