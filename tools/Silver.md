# Silver

## Links
- http://melt.cs.umn.edu/silver/

## Description

Silver [1] is an extensible attribute grammar specification system
that we have developed to investigate highly-modular language
specifications.

Silver supports many extensions to D. E. Knuth's
original specification of attribute grammars.  These include:

* *Forwarding* is useful in defining modular language extensions
  because it allows some aspects of a new constructs semantics to be
  specified implicitly via a translation to the host language.
  Semantics can be explicitly specified using traditional attribute
  definitions [2].

* *Higher-order attributes* as defined by Vogt and a form of
  *reference attributes* that are similar in spirit to those in 
  [[JastAdd]](http://jastadd.org/web/).

* *Collection attributes* a simplified version of those specified by
  Boyland are also supported in Silver. 

* *Pattern matching* over syntax trees.  This is similar to
  what is found in ML and Haskell.  Syntax trees correspond to
  algebraic data type values and productions correspond to value
  constructors [3]. 

* *Parametric Polymorphism* similar to what is found in ML and
  Haskell. Familiar syntax for polymorphic lists is also provided. 


Silver also has a modular analysis [4,5] that can be
used in extensible languages to ensure that the composition of
independently developed language extensions (that all pass this
analysis) compose to form a *well-defined* attrbribute grammar.

Silver comes bundled with [Copper](Copper.html), our parser and
context-aware scanner generator.

## Examples

```
grammar Arithmetic;

nonterminal Expr with value, unparse, env;

synthesized attribute value :: Integer;
syntehsized attribute unparse :: String;
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

1. The largest Silver specification is [[ableC]
   http://melt.cs.umn.edu/ableC/] [6], an extensible specification of C
   at the C11 standard.  Several modular language extensions to this
   have also been written.  

2. The Oberon0 specification[7] that was part of the LDTA 2011 Tool
   Challenge.

3. The RING language for chemical reaction networks [8].

## Key sources

1. Eric Van Wyk, Derek Bodin, Jimin Gao, and Lijesh Krishnan.
   Silver: an extensible attribute grammar system.
   *Science of Computer Programming*.
   75 (1-2): 39-54, January, 2010.
   [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/vanwyk10scp/).
   [[Publisher version]](https://doi.org/10.1016/j.scico.2009.07.004).

2. Eric Van Wyk, Oege de Moor, Kevin Backhouse, and Paul Kwiatkowski.
   Forwarding in Attribute Grammars for Modular Language Design.
   *Proceedings of 11th Conference on Compiler Construction* (CC 2002),
   LNCS, vol. 2304, pp. 128-142, Springer Verlag, 2002. 
   [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/vanwyk02cc/).
   [[Publisher version]](https://doi.org/10.1007/3-540-45937-5_11).

3. Ted Kaminski and Eric Van Wyk.
   Integrating attribute grammar and functional programming language features.
   *Proceedings of 4th the International Conference on Software Language
   Engineering* (SLE 2011), LNCS, vol. 6940, pp. 263--282, Springer
   Verlag, 2011.  
   [[Author pre-print]]( http://www-users.cs.umn.edu/~evw/pubs/kaminski11sle/).
   [[Publisher version]](https://doi.org/10.1007/978-3-642-28830-2_15).

4. Ted Kaminski and Eric Van Wyk.
   Modular well-definedness analysis for attribute grammars.
   *Proceedings of 5th the International Conference on Software
   Language Engineerin*g (SLE 2012), LNCS, vol. 7745, pp. 352--371,
   Springer Verlag, 2012.  
   [[Author pre-print]]( http://www-users.cs.umn.edu/~evw/pubs/kaminski12sle/).
   [[Publisher version]](https://doi.org/10.1007/978-3-642-36089-3_20).

5. Ted Kaminski. 
   Reliably composable language extensions. 
   Ph.D. Dissertation, Department of Computer Science and Engineering,
   University of Minnesota, Minneapolis, Minnesota, USA. July, 2017. 
   [[Archived version]](http://hdl.handle.net/11299/188954).

6. Ted Kaminski, Lucas Kramer, Travis Carlson, and Eric Van Wyk.
   Reliable and Automatic Composition of Language Extensions to C: The
   ableC Extensible Language Framework. 
   *Proceedings of ACM Programming Languages* 1, OOPSLA, Article 98
   (October 2017), 29 pages.  
   [[Author pre-print]]( http://www-users.cs.umn.edu/~evw/pubs/kaminski17oopsla/index.html).
   [[Publisher version]](https://doi.org/10.1145/3136040.3136055).

7. Ted Kaminski and Eric Van Wyk.
   A Modular Specification of Oberon0 Using the Silver Attribute
   Grammar System. 
   *Science of Computer Programming*.
   114 (C): 29--40, November, 2015.
   [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/kaminski15scp/).
   [[Publisher version]](https://doi.org/10.1145/3138224).

8. Srinivas Rangarajan, Ted Kaminski, Eric Van Wyk, Aditya Bhan, and
   Prodromos Daoutidis. 
   Language-oriented rule-based reaction network generation and analysis:
   Algorithms of RING
   *Computers and Chemical Engineering* 96 : 124--137, May, 2014.
   [[Publisher version]](https://doi.org/10.1016/j.compchemeng.2014.02.007).
