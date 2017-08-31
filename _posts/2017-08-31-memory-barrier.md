---
layout: post
title: Memory barrier
description: A summary of my understanding of memory barrier
---

I recently came across the term **memory barrier**. Several hours of research later, I think I have
a basic understanding.

First, some background information. Due to the speed differences between CPU and main memory,
hardware engineers employ many tricks to overcome the latency of accessing main memory.
One of those tricks is to reorder memory accesses.

As a Java developer, I used to think that the computer executes my code in the exact order as
specified in my program. Nothing can't be further from the truth. First of all, the Java compiler
may re-arrange my code for optimization reason. As a matter of fact, any optimizing compiler will do
that. Secondly, even the machine code may not be executed in the order as specified by the
compiler. This is called out-of-order execution.

How the CPU performs out-of-order execution is beyond the scope of my research. Suffice it
to say that the CPU can re-order memory accesses
as long as it does not change the outcome of a single-threaded program running in that CPU.

To illustrate, let's say a processor is executing the following machine code:

Processor 1 |
--- |
mov 1,[X] |
mov [Y], r1 |

The machine code `mov 1,[X]` means storing a value of 1 into memory location X.
The machine code `mov [Y], r1` means loading memory location Y into register r1.

In this example, it is perfectly legal for the processor
to execute `mov [Y], r1` before `mov 1,[X]`.
As far as the individual processor is concerned, the outcome of the program
has not been changed.

In order to force the processor to execute `mov 1,[X]` before `mov [Y], r1`,
we need to tell the processor to not reorder
memory access. The way to do that is to use a memory barrier.

Memory barrier is a hardware machine code that tells the CPU to enforce memory
access ordering around the barrier instruction.
Note that its effect is limited only to the CPU on which it is executed.
Memory barrier can be divided into three categories: store barrier, load barrier,
and full barrier.

A store barrier forces all store instructions before the barrier to
happen before the barrier and make the memory state of the CPU visible to other CPUs
(store means transmitting a value to memory or cache).

A load barrier forces all load instructions after the barrier to
happen after the barrier and make the memory states from other CPUs visible
to this CPU (load means transmitting a value from memory or cache).

A full barrier is a composite of both load and store barriers.

In the previous example, how do we force the processor to execute `mov 1,[X]`
before `mov [Y], r1`? By inserting a store barrier as follows:

Processor 1 |
--- |
mov 1,[X] |
store barrier |
mov [Y], r1 |

The store barrier forces the `mov 1,[X]` instruction to happen before the barrier,
thus achieving what we want.

In summary, memory barriers are hardware machine code instructions that force
the CPU to suspend memory access reordering at specified location in the program.
