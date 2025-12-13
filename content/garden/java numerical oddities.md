---
title: "Java Numerical Oddities"
date: 2023-12-24
lastmod: 2025-12-12
draft: false
garden_tags: [ ]
summary: "A few things I have noticed while working in java"
status: "growing"
---

In the course of working with Java, I've occasionally been surprised by some
behavior around numbers. This page collects the cases of that which I have found
most noteworthy.  These aren't bugs.  They're documented and (presumably)
intentional choices by the Java language developers.  I just find them
counter-intuitive. 

# The intValue method
The `Number` class provides a set of type conversion functions, including
`intValue`, the documentation for which says "may involve rounding or
truncation".  This is, perhaps, a bit understating the case. The documentation
for the `Long` implementation of `intValue` describes it as a "narrowing
primitive conversion", and directs the reader to the [Java Language
Specification](https://docs.oracle.com/javase/specs/jls/se7/html/jls-5.html#jls-5.1.3)
(it's always a fun time when you find yourself reading the actual language
specification.) The spec defines a narrowing primitive conversion on integral
types as follows:

> A narrowing conversion of a signed integer to an integral type T simply discards all but the n lowest order bits, where n is the number of bits used to represent type T. In addition to a possible loss of information about the magnitude of the numeric value, this may cause the sign of the resulting value to differ from the sign of the input value. 

For values in the normal range of an integer (that is, `-2^31` to `2^31 - 1`),
this behaves in an expected way - it just returns that integer.  For values
outside of that range, I find the behavior surprising to the point of
uselessness.  For example, `Long.MIN_VALUE.intValue() == 0` while
`Long.MAX_VALUE.intValue() == -1`[^1].

To make matters worse, `Double.intValue()` also performs a "narrowing primitive
conversion", but that means something different when moving between floating
point types and integral types.  You can read the full spec from the link above
(it's too long to quote), but the upshot is that out of range floating point
conversions saturate the integer. So by routing through `Double`, we get what I
consider more accurate results: `Double.valueOf(Long.MAX_VALUE).intValue() ==
Integer.MAX_VALUE`.

To me, the way the behavior varies between `Long` and `Double` makes `intValue`
an untrustworthy method. A function accepting a `Number` should not have to care
what implementation of `Number` it was passed, but without that information the
behavior of `intValue` is, essentially, undefined. 

# Floating Point Negative Zero
The IEEE standard for floating points
([IEEE-754](https://ieeexplore.ieee.org/document/8766229)) allows for negative
zeros.  It also specifies that, in general, the two zeros should compare as
equal. Java honors this, with `==`, `<=`, `>=`, etc all doing the right thing.
However, `Double.equals()` does not follow this convention. This behavior is
[documented](https://docs.oracle.com/javase/6/docs/api/java/lang/Double.html#equals%28java.lang.Object%29),
if it occurs to you to go read the docs for `Double.equals()`.  The given reason
for this is to allow hash tables to operate properly, and indeed it is
consistent with `Double.hashCode()` (as one would expect).  I don't know why
they didn't write `Double.hashCode()` such that negative and positive zero hash
to the same value though.

I should also note that the [documentation in Java
17](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Double.html)
is much clearer on this point, and appears at the top of the `Double` class docs
rather than just on the `Double.equals()` method.

Square root and pow also behave inconsistently with regard to negative zero.
The [docs](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html#sqrt-double-)
specify that `Math.sqrt(-0d) == -0.0`, but `pow(-0d, 0.5) == 0.0`.  This may be
documented in the tangled mess of edge cases that the 
[pow documentation](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html#pow-double-double-)
attempts to explain, but I still find the inconsistency surprising. As far as I
am aware, this is the only value `x` such that `sqrt(x) != pow(x, 0.5)` in
java.

# Non-Arabic Numerals
Credit to my colleague Parker for finding this one.  He filed an
[Elasticsearch Bug](https://github.com/elastic/elasticsearch/issues/133490)
about this, although it's a behavior in java.

`Long` and `Double` have different methods for parsing strings to numbers,
unsurprisingly.  What is somwhat surprising however is that they treat
non-(Western)Arabic numerals differently.  `Long.parseLong(String)` (and
`Long.valueOf(String)`, which is documented as having the same behavior)
seems to handle Unicode numerals that are not Western Arabic just fine.  I
tested it with an [Eastern
Arabic](https://en.wikipedia.org/wiki/Eastern_Arabic_numerals) number and a
[Javanese](https://en.wikipedia.org/wiki/Javanese_numerals) number.  I didn't
delve too deeply into these writing systems, and it's possible a native
speaker (writer?) could find places where `Long.parseLong()` doesn't behave
corectly, but at least for trivial cases it works.

Not so for `Double.parseDouble()`.  In the case of Doubles, any writing
system other than Western Arabic Numerals results in a number format
exception.  I assume this is because doubles are just harder to write
(indeed, the
[documentation](https://docs.oracle.com/en//java/javase/16/docs/api/java.base/java/lang/Double.html#valueOf(java.lang.String))
defines an entire grammar for specifying a double).  The same document goes
on to recommend the use of `NumberFormat` for interpreting "localized string
representations", and while I didn't test that, I have no reason to believe
it wouldn't work as advertised.

Like the negative zero behavior, I don't think this is wrong as such.
`NumberFormat` is a sensible way to handle localization, and for better or
worse Western Arabic numerals are the standard today.  Should longs provide a
level of leniency that isn't possible for doubles? I don't know.  It's not an
obviously wrong choice (unlike `IntValue`, :shakes fist at the sky:), but it
is a surprising choice.  And that's what I'm collecting here, things that
surprised me about numbers in Java.

[^1]: The max and min value constants are actually primitive longs, so this
    doesn't compile without boxing them.  I've left that out for ease of reading.
