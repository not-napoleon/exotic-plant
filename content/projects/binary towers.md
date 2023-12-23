---
title: "Binary Towers of Hanoi"
date: 2023-09-02
draft: false
project_tags: [ ]
status: "evergreen"
summary: "The Towers of Hanoi puzzle is just a binary counter"
links:
    external_link:
        text: "Python Code"
        icon: "fab alt brands fa-github"
        href: "https://gist.github.com/not-napoleon/812279c1491fcaeb36643e878ee03c5e"
        weight: 1
---

I've always enjoyed the Towers of Hanoi puzzle, and the recursive solution is
one of the first things I write when learning a new programming language. I
thought it would be fun to solve the problem using only bit manipulation.
Using this method, it is possible to compute the Nth move (assuming perfect
play) without having to calculate any moves before it - all the necessary
information is encoded in the number N.  In fact, one could describe this as
"stateless" Towers.

Essentially, what ever bit would be set when incrementing from N-1 to N, is
the same disk that will be moved on step N.  It's easy to see that every odd
numbered move will move disk 1 (the smallest disk) in this scheme, and that
any given disk will not be moved until all the disks above it have been, i.e.
that all the bits to the right are set to 1.

Once you have that intuition, it's just bit math.  I implemented it in python,
linked above.  
