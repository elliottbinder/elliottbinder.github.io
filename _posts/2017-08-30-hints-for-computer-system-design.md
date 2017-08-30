---
layout: post
title: "4 - Hints for Computer System Design"
date: 2017-08-30
author: Butler W. Lampson
source: https://www.microsoft.com/en-us/research/publication/hints-for-computer-system-design/
---

*This paper was published in 1983 so the systems mentioned are dated, but should still serve as valuable examples.  Our hindsight can emphasize the importance of certain design points and what affects they had on "future" (our past) system design.*

Through a series of design tales and *Hamlet* quotes, Lampson summarizes each hint with a slogan and categorizes each hint by *why* it helps making a good system (functionality, speed, or fault-tolerance) and *where* in the system it helps (ensuring completeness, choosing interfaces, or devising implementations).

Since this list is rather exhaustive, I'd have to agree with Lampson when he says that reading this can be tiresome and "perhaps it is best in small does at bedtime."

## Functionality

most of these hints have to do with interfaces. defining interfaces is the most important part of system design. good interfaces should be simple, complete, and admit a sufficiently small and fast implementation (these conflict).

#### Keep it simple

*Do one thing at a time, and do it well.*  This reminds me of the projects I've worked on that grew a little out of hand.  Especially later on as my programming experience grew and I valued "good design," it's always painful to pollute beautifully written code with something hacky just because no beautiful solution comes to mind when a new feature or function is needed.
I am also reminded of the common maxim I first remember hearing from my computer architecture class: spend most of your effort to optimize for the common case.  In this context, don't spend time optimizing an interface that isn't used widely.  If it is used widely, the optimization effort will be repaid many times over.

*Get it right.*  The example here points out the danger of simplicity and abstraction and to pay attention to the costs of the interfaces you are using, e.g. don't use an existing function for simplicity if it dramatically increases the time or space complexity unnecessarily.

#### Corollaries

*Make it fast,* rather than general or powerful.  This leaves the client the freedom to program what it wants and doesn't have to perform more work if it wants something different than some complex function you've implemented.  This immediately makes me think of RISC over CISC processor design, but nowadays general processor clocks can't run fast enough to make RISCs worthwhile in comparison.  Without all the complexity in today's x86 processors, you could fit many more cores onto a chip, but then you have to deal with the overhead of distribution and synchronization... but this sort of thing has made GPUs successful (given the specific problem domain).


*Don't hide power.*  This is similar to the last hint.  The purpose of abstraction is to conceal *undesireable* properties; powerful functionality should not be buried for the sake of generality -- allow the client to use this if it will be significantly faster.


*Use procedure arguments.*  E.g. allowing a filter to be passed to an iterator.  This allows the client plenty of flexibility without the need for an amalgamation of different parameters.  It may be better to have a specialized language, such as the case where static analysis leaves room for optimization e.g. database query languages.

*Leave it to the client* as long as it's inexpensive to pass control.  Solving one problem and stepping out of the way allows for simplicity, flexibility, and high performance.

#### Continuity

*Keep basic interfaces stable.*  A change to any interface that is widely used would then require changes to all the code that relies on that interface.  This can incur an unbearable cost, so such interfaces must be stable or risk system failure.

*Keep a place to stand* if the interface changes.  For example, if you want to support programs for an older architecture, you can emulate the instructions on your new machine.  Other examples include virtual machines and world-swap debuggers.

#### Making implementations work

*Plan to throw one away.*  Since there is a very good chance you'll have to change your implementation in the future, write it as a prototype -- acceptlably small, fast, and maintainable.

*Keep secrets* of the implementation i.e. assumptions about an implementation that client programs are not allowed to make.  In contrast, making assumptions generally can improve performance (the knowledge that an array is sorted allows for faster look-up) so striking a balance is an art.  The first example that comes to mind are reserved bits in machine instructions, which allow for future extension.

*Divide and conquer* by reducing a hard problem into several easier ones, e.g. processing large data sets in chunks to allow for pipelining or to express high locality.

*Use a good idea again* instead of generalizing it, e.g. for reliability, we can replicate data across multiple machines and again replicate the data within those machines.

#### Handling all the cases

*Handle normal and worst cases separately*: the normal case should be fast, and the worst case must make some progress.  It is okay to schedule processes unfairly or to deadlock the system, as long as this is detected and doesn't happen too often.  Even if processes or the entire system crashes, this may be a cheap price to pay for improved performance.

## Speed

*Split resources* in a fixed way if in doubt.  This is typically much simpler and faster, although the resource requirements will likely increase.  This will often be at a lower cost than the overhead stemming from multiplexing and fragmentation.   For example, when allocating virtual memory, pages are typically used instead of allocating from a pool so fragmentation isn't an issue.

*Use static analysis* if you can.  This usually involves using knowledge of the system to change the way a program is executed to improve runtime.  There are many examples involved in compiling programs, from constant folding to register allocation to accessing memory in a pattern recognizable by the hardware.

*Dynamic translation* from a convenient representation to one that can be quickly interpreted is a variation on compilation.  Dynamically translating java bytecode to machine instructions for any architecture a program is running on allows for more portability and doesn't require translating source code that won't be used.

*Cache answers* to expensive computations.  As long as the function provides consistent output for each input, the overhead of caching the results can be less than recomputing.  This can also be used when updating values e.g. the sum of a large array can be saved and recomputed when another element is added without having to re-sum the entire array.

*Use hints* to speed up normal execution.  Hints are like cache entries in the way that they save computation, but with distinct differences: the hint can be wrong and is not necessarily reached through associative lookup.  Hints must be easily verifiable for correctness, which should cost less than computing the value without the hint.  An example would be the store and forward routing first used in the Arpanet: each node in the network keeps a table of what it thinks is the best route to each node.  Periodically, each node will broadcast its opinion of the quality of the links to its neighbors so nodes can update their tables.  Although there isn't any synchronization, this behaves nearly optimally when routes don't change too fast.

*When in doubt, use brute force.*  The success of personal computers over time-sharing systems required much less cleverness at the cost of many wasted cycles.  Another example is using linear search on a very small array may not be noticeably different than using a fancy data structure.

*Compute in background* when possible.  In interactive or real-time systems, response times are most important but other things may be less important so work can be done in the background otherwise.  For example, paging systems typically write out dirty pages in the background.  Email can be delivered and retrieved by background processes since delivery isn't all that time-critical.

*Use batch processing* if possible.  Doing things incrementally almost always costs more, even aside from the fact that writing to disk works better when writing sequentially.  Many banks process and finalize transactions during a nightly batch run.

*Safety first.*  In allocating resources, prioritize avoiding disaster over optimization.  A system can easily be overloaded and without proper safety precautions, service can be drastically degraded.  For example, design for paging systems initially tried to squeeze out as much optimization as possible (putting related procedures on the same page, predicting the next pages to be referenced, running jobs together that share data or code) but memory got cheaper and systems spent it to provide enough cushion for simple demand paging to work.  Again, spend optimization efforts where it counts most.

*Shed load* to control demand, rather than allowing the system to become overloaded.  This can be done through refusing service to new users, or denying service to existing ones.  If a system becomes overloaded, all users may experience loss of service.

## Fault-tolerance

*End-to-end.*  Error recovery at the application level is absolutely necessary for a reliable system, and any other error detection or recovery is not logically necessary but is strictly for performance.  For example, consider transferring a file from machine A to machine B.  You can compute a checksum of reasonable length (say 64 bits) on each machine and compare for correctness.  Doing this at any intermediate step (over the network, from memory to disk, or vice-versa) is not sufficient, since any other step may result in error.  But introducing checks into these steps can improve performance e.g. including a checksum in each packet set over the network and resend just that packet if incorrect.  Some problems come along with implementing the end-to-end strategy: it requires a cheap test for success, and severe performance defects may not creep up until the system is in place and under heavy load.

*Log updates.*  To use this technique, record every update to an object as a log entry consisting of the update procedure and its arguments.  The procedure must be *functional* (same inputs always have the same outputs) i.e. there is no state outside of the arguments that affects the operation of the procedure.  Then, the log entry can be re-executed later to reach the same state.

*Make actions atomic or restartable.*  Sometimes called *transactions*, atomic actions either complete fully or have no effect.  This ensures no in-between and invalid state in the case of a failure mid-execution.  For example, a database system may have a commit record that can be used for recovery in the case of a failure.  A failed action can be re-executed without any worry that the system state has changed, without the need to roll back to a previous state.
