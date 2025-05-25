---
name: weechess-rs
github: weechess-rs
description: A slightly better chess engine, in Rust
layout: project
---

```
$ cargo run --release 

Usage: weechess [COMMAND]

Commands:
  display   Print out the board in a human-readable format
  evaluate  Evaluate a position
  perft     Count strictly legal moves to a certain depth
  repl      Start an interactive REPL session with the engine
  uci       Start a UCI client
  version   Print out the version of the engine
  help      Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

## About

[Previously, I built a chess engine](/projects/weechess.html).

It was pretty good, but there were some design decisions I had made that had become less
than ideal once I started trying to optimize search. For example, I had no concept of a
pseudo-legal move, which is cheaper to generate than a strictly legal move because you
don't need to check if making that move would put the king in check. If you can generate
pseudo-legal moves then you can include them in the search tree, only promoting them to
a strictly legal move if you actually want to search that branch. 

In addition to wanting to improve the performance and design of the engine, I also wanted
to learn Rust, and this was a great opportunity to do so, having a pretty good idea of what
I wanted to build.

## Improvements

This version of the engine turned out measurably better than the previous one. It supports
all the [search features of the previous engine](/projects/weechess.html#search), plus
some more nifty improvements:
 - **Quiescence Search**: This is a technique that allows you to search only the most important moves
   in a position. This allows you to search interesting positions deeper.
 - **Iterative Deepening**: This is a technique that allows you to search the tree in a depth-first manner,
   one ply at a time. This seems like it would be wasteful becaues you'd have to re-search the tree for
   depths that have already been searched, but because of the Transposition Tables, this is not a problem.
   In fact, it allows you to more efficiently search because you can choose to search the most promising
   branches from previous searches first, increasing the effectiveness of alpha-beta pruning.
 - **Multithreading**: Using a variation of an algorithm called [Lazy SMP](https://www.chessprogramming.org/Lazy_SMP),
   search can be parallelized. This is a really cool algorithm -- the way it works is by simply spawning
   multiple threads to search the same tree. By introducing a bit of randomness into the order of moves
   to search, each thread will search different branches of the tree, leveraging the transposition tables to
   avoid searching the same branches. Adding buckets to the transposition table protected by read-write locks
   helps avoid most of the thread contention.

This engine spanks the previous one in games. I also setup some tooling to compare versions of the engine
against eachother, which has helped me tweak and improve the evaluation functions some.

## Thoughts on Rust

There are no novel takes on Rust anymore, but I will share mine:
 * the tooling is phenomenal
 * the ecosystem is coherent, consistent, and well documented
 * the error messages are the best I've ever seen
 * modelling the problem domain is much much easier than in _some languages_
 * it feels a lot easier to design correct interfaces than in _some languages_
 * it feels a lot easier to build working implementations than in _some languages_
 * the borrow checker works

It took me a few tries the really get the hang of Rust, but hey, it took me a few tries to get into 
The Office (US) too.
