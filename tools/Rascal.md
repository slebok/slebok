# Rascal Metaprogramming Language

## Links
* http://www.rascal-mpl.org
* http://bibtex.github.io/tag/rascal.html
* https://stackoverflow.com/questions/tagged/rascal

## Description
a one stop shop for metaprogramming

uses a variant of [GLL](../terms/GLL_parser.md)

## Example
Grammar:
```Rascal
module Syntax

extend lang::std::Layout;
extend lang::std::Id;

start syntax Machine = machine: State+ states;
syntax State = @Foldable state: "state" Id name Trans* out;
syntax Trans = trans: Id event ":" Id to;
```

Checker:
```Rascal
module Analyze

import Syntax;

set[Id] unreachable(Machine m) {
  r = { <q1,q2> | (State)`state <Id q1> <Trans* ts>` <- m.states, 
				  (Trans)`<Id _>: <Id q2>` <- ts }+;
  qs = [ q.name | /State q := m ];
  return { q | q <- qs, q notin r[qs[0]] };
}
```

## Key sources
* http://dx.doi.org/10.1109/SCAM.2009.28
* http://dx.doi.org/10.1007/978-3-642-18023-1_6

## Ownership
* Originally by @paulklint, @jurgenvinju & @tvdstorm
* Mainly developed and maintained by the SWAT team at CWI
