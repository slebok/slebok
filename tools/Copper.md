# Copper

## Links
- http://melt.cs.umn.edu/copper/

## Description

Copper is a parser and scanner generator that generates integrated LR parsers and context-aware scanners from language specifications based on context-free grammars and regular expressions. The unique feature in Copper is that the generated parser provides contextual information to the scanner in the form of the current LR parse state when the scanner is called to return the next token. The scanner uses this information to ensure that it only returns tokens that are valid for the current state, that is, those for terminals whose entry in the parse table for the current state is shift, reduce, or accept (but not error). Context-aware scanners are more discriminating than those that lack context and allow the specification of simpler grammars that are more likely to be in the desired LALR(1) grammar class.

## Examples

    TODO

## Key sources

- http://www-users.cs.umn.edu/~evw/pubs/vanwyk07gpce/
- http://www-users.cs.umn.edu/~evw/pubs/schwerdfeger09pldi/
