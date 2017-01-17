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

## {data-background="images/simples.jpg" data-background-size="auto 100%" data-background-color="white"}

---
# reveal.js settings
theme: night
transition: concave
backgroundTransition: zoom
center: true
history: true
css: custom.css
...
