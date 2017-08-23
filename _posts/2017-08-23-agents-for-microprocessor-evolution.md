---
layout: post
title: "2 - Requirements, Bottlenecks, and Good Fortune: Agents for Microprocessor Evolution"
date: 2017-08-23
author: Patt, Yale
source: http://www.ece.cmu.edu/~ece447/s12/lib/exe/fetch.php?media=wiki:18447-patt-2001.pdf
---

Patt identifies that microprocessor design requires trade-offs pivoting around "design points" -- characteristics that are most important to the specific use of the microprocessor, such that other characteristics can be compromised.  Characteristics such as performance, cost, heat dissipation, power consumption, high availability, low power or energy consumption may combat when considering the primary use of the processor. For example,

* scientific computation values high performance above features like precise exception handling
* e-commerce businesses require transactional execution and reliability, but are willing to pay high costs
* low power processors may prioritize saving energy over performance

The Agents of Evolution are the primary driving forces for advancement in microprocessor design.

**New requirements** are the ever growing hunger from the market to desire bigger and better things after taking advantage of all the improvements offered in the past.
* higher performance requirements lead to additional functional units on chip and increasing the number of instructions fetched per cycle
* lower power consumption has been achieved through improved manufacturing processes and by adjusting the rate at which some functional units operate
* improving the human interface as computer/human interaction becomes more pervasive

**Bottlenecks** usually arise through one of the three components of instruction processing (instruction supply, data supply, and execution).
* slow memory lead to the invention of caches
* stalls in execution by conditional branches lead to branch prediction
* stalls in execution by data availability latency lead to out-of-order execution

**Good fortune** is when something causes a windfall that allows additional functionality to be added to the chip.
* the ever shrinking size of transistors allows for more space on the chip to be utilized for floating point units or multimedia instruction extensions.

Patt goes on to explain the evolution of microprocessors from 1971 to 2001 (when the paper whas published) including pipelining, on-chip caches, branch prediction, on-chip specialized functional units, out-of-order processing, clusters, chip multiprocessors, simultaneous multithreading, and fast cores.  He then looks to the future to enumerate several features that will be present in microprocessors, such as
* microprocessors that are accessible and configurable to users higher in the computation hierarchy -- for example, reconfigurable logic could be placed on the chip so that an application could customize the hardware to efficiently execute its own algorithms.
* the growing length of wires and frequency of clocks make signals more susceptible to error, so changing data paths may be needed
* such errors could be detected or corrected internally to improve fault-tolerance
* expanded use of microcode could effectively harness the plentiful on-chip bandwidth

Patt closes with an optimistic look at the future of microprocessor design, as many naysayers in the past have declared the end of Moore's law but improvements prevail.  It takes quite an experienced and knowledgeable architect such as himself to clearly state the artful design that goes into microprocessors.  Even though I am still early in my career, I can identify that many of the features listed have been implemented in modern processors.  I'm afraid I am not experienced enough to say whether any of these features are not realistic, but I'm sure time will tell.
