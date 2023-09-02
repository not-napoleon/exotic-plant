---
math: true
title: "Sums of Cubes"
date: 2023-09-02
lastmod: 2023-09-02
draft: false
garden_tags: [ ]
summary: "A proof by induction of a well known equality"
status: "evergreen"
---

It is well known that the sum of the first N integers, squared is equal to 
the sum of the first N cubes, and there are [many proofs](https://en.wikipedia.org/wiki/Squared_triangular_number)
of this already. The geometric proof is both famous and beautiful.  I,
however, am a programmer, and as such, my first instinct is to prove by
induction.  I'm sure I'm not the first to do this, but I had fun working it
out for myself.

So, formally, I want to show that 
$$ \sum_{i=1}^n i^3 = \lparen \sum_{i=1}^n i \rparen ^2 $$

First, demonstrate that this is true for n=1
$$ \sum_{i=1}^1 i^3 = \lparen \sum_{i=1}^1 i \rparen ^2 $$
$$ 1^3 = 1^2 $$

Then the inductive step: assuming that it is true for some N, prove that it is
true for N+1:

Given 
$$ \sum_{i=1}^n i^3 = \lparen \sum_{i=1}^n i \rparen ^2 $$
prove 
$$ \sum_{i=1}^{n+1} i^3 = \lparen \sum_{i=1}^{n+1} i \rparen ^2 $$
Rewriting the sum back to the original range yields
$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \lparen \sum_{i=1}^n i \rparen + n + 1 \rparen ^2 $$
expand out the square to get
$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \lparen \sum_{i=1}^n i \rparen + n + 1 \rparen \lparen \lparen \sum_{i=1}^n i \rparen + n + 1 \rparen  $$
and multiply to get

$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \sum_{i=1}^n i \rparen ^2 + \lparen 2n + 2 \rparen \lparen \sum_{i=1}^n i \rparen + n^2 + 2n + 1$$

Next, apply two identities; The triangular number identity says that
$$ \sum_{i=1}^n = n(n+1) \over 2 $$ 

and factor to get

$$ n^2 + 2n + 1  = (n+1)^2 $$ 

rewriting again to get

$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \sum_{i=1}^n i \rparen ^2 + \lparen 2n + 2 \rparen \lparen {n(n+1) \over 2} \rparen + (n+1)^2 $$

and simplify
$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \sum_{i=1}^n i \rparen ^2 + (n+1) \lparen {n(n+1)} \rparen + (n+1)^2 $$
$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \sum_{i=1}^n i \rparen ^2 + n(n+1)^2 + (n+1)^2 $$
$$ \lparen \sum_{i=1}^n i^3 \rparen + (n+1)^3 = \lparen \sum_{i=1}^n i \rparen ^2 + (n+1)^3 $$

From our initial inductive assumption, we have 
$$ \sum_{i=1}^n i^3 = \lparen \sum_{i=1}^n i \rparen ^2 $$
and clearly 
$$ (n+1)^3 = (n+1)^3 $$
so that concludes the proof.  This shows that if the initial hypothesis is
true for some N, it is also true for N+1, and demonstrates that it is true for
N=1. Thus, by induction, it is true for all N > 1.
