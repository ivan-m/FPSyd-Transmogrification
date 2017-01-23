---
title: Transmogrify your data!
author: Ivan Lazar Miljenovic
date: 25 January, 2017
...

Haskellers, we have a problem!
==============================

## We think our language is all nice and theoretical...

> * "Look at me, I use category theory!"
> * "This is all based off of research, not just baseless pragmatism!"

## We love our types!

```haskell
data Foo = Foo Int Double Bool

-- See? So much clearer and safer and stuff!
```

## But this can lead to a problem...

> "I have a `Foo`... but this function requires a `Bar` which is
> **identical** to a `Foo` just with a different name...

## ... because we forgot some theory!

> In type theory a product type of two types `A` and `B` is the type
> whose terms are ordered pairs `(a,b)` with `a:A` and
> `b:B`.

Notes
:   * Similarly for sum types.

## Data definitions complicate things!

Notes
:   * All that extra type information just gets in the way!

The Solution
============

## If people continue to complicate matters by writing new types everywhere...

## We'll just have to strip all that metadata out!

## {id="simples" data-background="images/simples.jpg" data-background-size="auto 100%" data-background-color="white"}

## You can't just _say_ "Simples"...

Notes
:   * Fine, you got me...
    * I do have an answer though!

`GHC.Generics`
==============

## A brief recap

. . .

Everything is either:

> * `K1`  -- An individual field/constructor
> * `V1`  -- Empty datatypes
> * `U1`  -- A constructor without fields
> * `:*:` -- Product types
> * `:+:` -- Sum types
> * `M1`  -- Pesky, pesky metadata...

Notes
:   * After all, they're so awesome that everyone knows and uses them,
      right?
    * We can convert instances of `Generic` to and from this
      representation with ease!

## So, what do we have to do?

> 1. Get everything to derive `Generic`
> 2. Convert it into it's generic representation
> 3. Recursively remove any `M1` wrappers present
> 4. Add in the `M1` wrappers that the other type wants
> 5. We have achieved...

TRANSMOGRIFI-CATION!!! {data-background="images/new.jpg" data-background-size="auto 100%" data-background-color="white"}
===================

## Example

```haskell
λ> data Foo a c = Foo a Int c
     deriving (Show, Generic)
λ> data Bar b = Bar { a :: Word
                    , b :: b
                    , c :: Char
                    }
     deriving (Show, Generic)
λ> transmogrify (Foo (3 :: Word) 2 'a')
     :: Bar Int
Bar {a = 3, b = 2, c = 'a'}
```

## {id="it's alive" data-background="images/alive.jpg" data-background-color="black"}

## But...

. . .

... it's a tad boring.

## What about nested examples?

> * Let's pretend there's no laziness.
> * Then the following are isomorphic:
>     - `('t', 'u', 'p', 'l', 'e')`{.haskell}
>     - `(('t', 'u'), 'p', ('l', 'e'))`{.haskell}
>     - `('t', ('u', ('p', ('l', ('e')))))`{.haskell}

Notes
:   * Can you tell I once did a bit of lisp?

## How to deal with this?

> * Recursively merge children product-types
> * Canonicalise the representation
> * Basically, convert everything into a lisp-style list:
        `'t' :*: ('u' :*: ('p' :*: ('l' :*: ('e'))))`{.haskell}

Notes
:   * Also sum-types
:   * Generics tries to be helpful and use semi-balanced trees... this
      isn't documented and can be annoying.

## Witness the power! {data-background="images/fully-powered.png" data-background-color="black"}

. . .


```haskell
λ> transmogrify
     ('H', 'a', 's', 'k', 'e', 'l', 'l')
     :: ((Char, Char)
        , Char
        , (Char, Char, (Char, Char)))
(('H','a'),'s',('k','e',('l','l')))
```

Notes
:   * Witness the power of this fully-powered transmogrifier!!!
    * 7-tuples are the largest tuples with a Generics instance

So, everyone should use this, right?
====================================

## There's no type inference or safety... {data-background="images/no-inference.gif" data-background-size="auto 100%" data-background-color="white"}

Notes
:   * Even worse inference than `read`, as you have to hope they line
      up
    * There's also `O(n)` performance, but you'd expect that.
    * But worst of all...

## How do you stop recursing?

. . .

```haskell
type family CanRecurse a :: Bool where
  CanRecurse Int     = 'False
  CanRecurse Int8    = 'False
  CanRecurse Int16   = 'False
  CanRecurse Int32   = 'False
  CanRecurse Int64   = 'False
  CanRecurse Integer = 'False
  -- Snip other numeric types
  CanRecurse Char    = 'False
  CanRecurse a       = 'True
```

Notes
:   * Non-extensible :(
    * Required because we need to determine this at the type-level,
      and there's no `isInstanceOf` function.

## So, ummm....

> * If it was just the performance and type-inference, this would
>     still be useful.
> * But without extensibility, this isn't really viable.
> * As such, this isn't on Hackage.
> * So all I can say is...

USE GENERICS {data-background="images/skills.jpg" data-background-size="auto 100%" data-background-color="white"}
============

---
# reveal.js settings
theme: night
transition: concave
backgroundTransition: zoom
center: true
history: true
css: custom.css
...
