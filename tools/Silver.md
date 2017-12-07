# Silver

## Links
- http://melt.cs.umn.edu/silver/

## Description

Silver is an extensible 
[attribute grammar](../terms/attribute_grammar.md) specification
system that we have developed to investigate highly-modular language
specifications.

Silver supports many extensions to D. E. Knuth's
original specification of attribute grammars.  These include:

* [Forwarding](../terms/attribute_forwarding.md)
  is useful in defining modular language extensions
  because it allows some aspects of a new constructs semantics to be
  specified implicitly via a translation to the host language.
  Semantics can be explicitly specified using traditional attribute
  definitions.

* [Higher-order attributes](../terms/higher-order_attribute.md)

* [Reference attributes](../terms/reference_attribute.md)

* [Collection attributes](../terms/collection_attribute.md)

* Pattern matching over syntax trees.

* Parametric polymorphism.

Silver also has a modular analysis that can be used in extensible
languages to ensure that the composition of independently developed
language extensions (that all pass this analysis) compose to form a
*well-defined* attribute grammar.

Silver comes bundled with [Copper](Copper.md), a parser and
context-aware scanner generator.

## Examples

```
grammar Arithmetic;

nonterminal Expr with value, unparse, env;

synthesized attribute value :: Integer;
synthesized attribute unparse :: String;
inherited attribute env :: [ Pair<String Integer> ];

abstract production add
e::Expr ::= l::Expr r::Expr
{
  e.value = l.value + r.value;
  e.unparse = "(" ++ l.unparse ++ " + " ++ r.unparse ++ ")";
  l.env = e.env;
  r.env = e.env;
}

abstract production sub
e::Expr ::= l::Expr r::Expr
{
  e.value = l.value - r.value;
  e.unparse = "(" ++ l.unparse ++ " + " ++ r.unparse ++ ")";
  l.env = e.env;
  r.env = e.env;
}

abstract production negation
e::Expr ::= n::Expr
{
  e.unparse = "( - " ++ n.unparse ++ ")" ;
  n.env = e.env;

  forwards to sub(const(0), n);
  {- The value of e.value is automatically copied from
     the forwards to tree. -}

abstract production const
e::Expr ::= i::Integer
{ 
  e.value = i;
  e.unparse = toString(i);
}

abstract production variable
e::Expr ::= v::String
{
  e.value = lookup(v, e.env);
  e.unparse = v;
}
```

## Applications

Three uses of Silver (and Copper) of interest are

1. The largest Silver specification is [ableC](http://melt.cs.umn.edu/ableC/),
   an extensible specification of C at the C11 standard.  Several
   modular language extensions to this have also been written.  

2. The Oberon0 specification that was part of the LDTA 2011 Tool
   Challenge.

3. The RING language for chemical reaction networks.

## Key sources

- Eric Van Wyk, Derek Bodin, Jimin Gao, and Lijesh Krishnan.
  Silver: an extensible attribute grammar system.
  *Science of Computer Programming*.
  75 (1-2): 39-54, January, 2010.
  [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/vanwyk10scp/).
  [[DOI]](https://doi.org/10.1016/j.scico.2009.07.004).

