# Kiama

## Links
- https://bitbucket.org/inkytonik/kiama

## Description

Kiama is a Scala library for language processing containing shallow embeddings of language processing formalisms including parser combinators, attribute grammars, strategy-based term rewriting and abstract state machines.

The key design feature of Kiama is that it makes these processing formalisms available within the normal realm of Scala programming, so users do not have to radically change their approach or rewrite existing code when adding Kiama-based code.

## Examples

* Attribute definitions for the classic RepMin problem to duplicate a binary tree structure while replacing numeric leaves with the minimum leaf value from the whole tree. Uses a synthesized attribute (`locmin` for local minimum), an inherited attribute (`globmin` for global minimum) and a higher-order attribute (`repmin` for the new tree). Also uses tree relations (`tree.parent`) to examine the structure.

```Scala
val locmin : RepminTree => Int =
    attr {
        case Fork(l, r) => locmin(l).min(locmin(r))
        case Leaf(v)    => v
    }

val globmin : RepminTree => Int =
    attr {
        case tree.parent(p) => globmin(p)
        case t              => locmin(t)
    }

val repmin : RepminTree => RepminTree =
    attr {
        case Fork(l, r) => Fork(repmin(l), repmin(r))
        case t : Leaf   => Leaf(globmin(t))
    }
```

* Circular attributes that evaluate to a fixed point to encode classical data-flow equations.

```Scala
val in : Stm => Set[Var] =
    circular(Set[Var]())(
        s => uses(s) ++ (out(s) -- defines(s))
    )

val out : Stm => Set[Var] =
    circular(Set[Var]())(
        s => succ(s) flatMap (in)
    )
```
* Rewriting strategies to implement innermost evaluation of lambda calculus terms.

```Scala
val beta =
    rule[Exp] {
        case App(Lam(x, t, e1), e2) => Let(x, t, e2, e1)
    }

val lambda =
    beta + arithop + ...

def innermost (s : => Strategy) : Strategy =
    all(innermost(s) <* attempt(s <* innermost(s)))

val eval : Strategy =
    innermost(lambda)
```

## Applications

A. M. Sloane, M. Roberts. *Oberon-0 in Kiama*. Science of Computer Programming 114 (2015) 20-32. [DOI](https://doi.org/10.1016/j.scico.2015.10.010)

## Key sources

A. M. Sloane. *Lightweight Language Processing in Kiama*. Tutorial at 3rd Summer School on Generative and Transformational Techniques in Software Engineering (GTTSE 2009). [DOI](https://doi.org/10.1007/978-3-642-18023-1_12)

A. M. Sloane, L. C. L. Kats, and E. Visser. *A pure embedding of attribute grammars*. Science of Computer Programming, 78(10), October 2013. [DOI](https://doi.org/10.1016/j.scico.2011.11.005)

A. M. Sloane, M. Roberts, and L. G. C. Hamey. *Respect Your Parents: How Attribution and Rewriting Can Get Along*. In Proceedings of the International Conference on Software Language Engineering (SLE 2014), 191-210. [DOI](https://doi.org/10.1007/978-3-319-11245-9_11)
