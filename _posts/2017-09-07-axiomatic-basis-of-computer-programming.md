---
layout: post
title: "5 - An Axiomatic Basis for Computer Programming"
date: 2017-09-07
author: C. A. R. Hoare
source: https://web.archive.org/web/20160304013345/http://www.spatial.maine.edu/~worboys/processes/hoare%20axiomatic.pdf
---

Hoare outlines the idea that computer programs can be formally proven to be correct in the same way as other fields of mathematics by defining axioms as a logical foundation and applying deductive reasoning[^1].  The paper lists a small set of axioms for arithmetic operations on finite integers[^2] and defines a simple programming language with axioms on *assignment*, *consequence*[^3], and *iteration*.  With these in place, we can deduce provably correct statements about the conditions of variables at the end (or at intermediate points) of program execution.  Often more than half of a program's development time comes from testing and debugging, which emphasizes the importance of proving correctness.

I've written plenty of incorrect code in my day, so the promise of provenly correct code is enticing.  I'm curious as to how much time would be needed to formally prove a program correct compared to traditional testing methods.  My guess would be that the complexity of proofs grows more quickly than that of programs, so they soon become intractable for programmers to reason about mentally.  Taking an approach of partitioning a program into smaller logical sections (much like Hoare's axiom of consequence) and proving each of these would be much easier to manage.

I also wonder how established axioms could inform the design of algorithms so that they are more naturally provable, rather than contorting the axioms to fit with whatever code you've already written.



[^1]: statements of the form *if axiom A and axiom B then some new axiom C*.
[^2]: *finite* due to the hardware constraints of computers.
[^3]: basically, if the post condition of one function *A* implies the pre condition of another function *B*, then the pre condition of *A* implies the post condition of *A*;*B*.
