# Copper

## Links
- http://melt.cs.umn.edu/copper/

## Description

Copper is a parser and scanner generator that generates integrated LR parsers and context-aware scanners from language specifications based on context-free grammars and regular expressions. The unique feature in Copper is that the generated parser provides contextual information to the scanner in the form of the current LR parse state when the scanner is called to return the next token. The scanner uses this information to ensure that it only returns tokens that are valid for the current state, that is, those for terminals whose entry in the parse table for the current state is shift, reduce, or accept (but not error). Context-aware scanners are more discriminating than those that lack context and allow the specification of simpler grammars that are more likely to be in the desired LALR(1) grammar class.

## Examples

    TODO

## Applications

Two primiary uses of Copper (and Silver) of interest are

1. The largest Silver specification is [ableC](http://melt.cs.umn.edu/ableC/),
   an extensible specification of C at the C11 standard.  Several
   modular language extensions to this have also been written.  

2. The Oberon0 specification that was part of the LDTA 2011 Tool
   Challenge.
   
## Key sources

- Eric Van Wyk and August Schwerdfeger.
  Context-Aware Scanning for Parsing Extensible Languages.
  *Proceedings of International Conference on Generative Programming and Component Engineering, (GPCE 2007)*,
  pp. 63-72. ACM Press, 2007. 
  [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/vanwyk07gpce/).
  [[DOI]](https://doi.org/10.1145/1289971.1289983)
  
- August Schwerdfeger and Eric Van Wyk.
  Verifiable Composition of Deterministic Grammars.
  *Proceedings of ACM SIGPLAN Conference on Programming Language Design and Implementation (PLDI 2009)*,
  pp. 199-210, ACM Press, 2009. 
  [[Author pre-print]](http://www-users.cs.umn.edu/~evw/pubs/schwerdfeger09pldi/).
  [[DOI]](https://doi.org/10.1145/1542476.1542499)
