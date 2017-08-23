# Program slicing

## Definition
A program transformation technique where all code that is irrelevant to a certain set of inputs or outputs is removed. Applied *forwards*, any code not directly or indirectly using a selection of inputs is discarded; applied *backwards*, all code that does not contribute to the computation of the selected outputs is discarded. Mainly used in debugging (e.g., to find the code that might have contributed to an error), but sometimes also as an optimisation technique. Originally formalised by Mark Weiser in the early 1980s.

## Links


[[Wikipedia](http://en.wikipedia.org/wiki/Program_slicing)]

## Tags
Transformation


