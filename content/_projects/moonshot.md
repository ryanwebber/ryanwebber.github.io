---
name: moonshot
github: moonshot
description: A (very limited) compiler toolchain targeting the Apollo Guidance Computer
layout: project
---

```
// Definition of a program that is invoked by
// typiing VERB 00 and NOUN 00 on the DSKY
prog {
    .verb = 00;
    .noun = 00;
    .entry = main;
}

// The entry point of a program. State is kept
// and restored between invocations of the program
// (i.e. between DSKY commands or system reboots)
state main () [
    a: WORD = 0;
] {
    a = add(a: a, b: 1);
}

// Basic subroutines
sub add(a: WORD, b: WORD) {
    return a + b;
}
```

## About

You know the Apollo Guidance Computer (AGC)? It's a flight computer that was used onboard
both the Command Module (the bit that orbited the moon) and Lunar Module (the bit
that landed on the moon) during the Apollo missions. This was a really impressive piece
of technology for the time, and its architecture is _crazy_. Seriously, read the
[Wikipedia article](https://en.wikipedia.org/wiki/Apollo_Guidance_Computer) for this thing.

Recently, the [Luminary source code](https://github.com/chrislgarry/Apollo-11) was released
into the public domain, and my complete inability to read the assembly language made me
wonder if there was a way to write programs for the AGC in a higher-level language --
could I write software to perform a simulated moon landing in my own language compiled
for the AGC? So, down the rabbit hole I went.

## The Setup

The [Virtual AGC](https://www.ibiblio.org/apollo/#gsc.tab=0) is an amazing project that
provides a complete simulation environment for the AGC, including the DSKY (the goofy display
and keyboard that the astronauts used to interact with the computer) as well as an
assembler for the AGC assembly language. So all the tools I needed to run my programs
were already available. I just needed to write a compiler that would take my code and
translate it into the AGC assembly language.

## The Compiler

The compiler has gone through a few iterations. The first version was way overengineered
and I scrapped it pretty quickly. The current design is a much more simple. It supports
a single 15-bit integer type (yes, really), arithmetic operations, and conditional
statements. The syntax is tightly bound to a task-based runtime model that would provide
scheduled code execution, similar to the way the actual AGC software worked (but this
runtime is not implemented yet). Additionally, the goal is to provide a simple standard
library that provides a few basic functions for interacting with the DSKY and other
components of the AGC.

As I mentioned, this whole thing is only partially implemented. I do have integration tests
stood up that can actually compile and run a simple programs on the Virtual AGC, which I
expect to rely on heavily as I continue to develop the compiler. I come back to this project
every few months because of how fascinating the AGC is, and I hope to eventually work my way
towards a complete compiler that can run more complex programs.
