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

---
# reveal.js settings
theme: night
transition: concave
backgroundTransition: zoom
center: true
history: true
css: custom.css
...
