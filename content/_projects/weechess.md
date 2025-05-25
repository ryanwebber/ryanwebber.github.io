---
name: weechess
github: weechess
description: A simple chess engine
layout: project
---

![Screenshot of Weechess](/assets/screenshots/weechess-01.webp)

## About

If you're looking for interesting software projects,
[Sebastian Lague](https://www.youtube.com/@SebastianLague) has
some great content. One of which is a series on building a chess engine. I'm terrible
at chess, but I found some of the concepts technically interesting, so I dove in.

## The Easy Bits

Building an implementation of the game of chess is somewhat straightforward. The data structures
are pretty obvious, and the rules are extremely well defined. With a bit of time
and effort, you can get the basic game running. In order ensure you've gotten all
the rules implemented correctly, it's quite convenient to use an existing chess engine
to validate legal moves by counting them up to a certain depth for a given position. I
found quite a few bugs in my implementation this way, mostly around castling.

Slap a terminal UI on top and you've got something pretty good! Although, you're going
to need a bot to play against...

## The Hard Bits

The next step is to implement the engine, and where this get obviously really hard. Many chess
engines are implemented by searching the game tree and using a heuristic to evaluate the position,
and this is the approach I took.

### Search

The first thing to implement is the search algorithm. Remember when I said that it's convenient 
to count all legal moves to a certain depth to validate our implementation? Well, that's the
starting point for the search algorithm. However, the trick to making search fast is to _avoid_
searching the entire tree. There are a few techniques to do this, and I won't go into each,
but these are the ones I implemented:
 - **Alpha-Beta Pruning**: This is a technique that allows you to skip searching branches of the tree
   that are guaranteed to be worse than the current best move. This is a huge win for performance.
 - **Transposition Tables**: This is a technique that allows you to store the results of previously
   searched positions and reuse them later.

So far we've focused on reducing our search space, but there's another way to speed up search: improve
search performance. There's a couple of techniques to do this that I implemented, and again I won't
go into too much detail for each:
 - **Bitboards**: This is a technique that allows you to represent the board as a single 64-bit integer. This
   allows you to use bitwise operations to quickly check for legal moves.
 - **Magic Number Tables**: This is a technique that allows you to quickly generate legal moves for a given
   piece. It's bit manipulation and arithmatic black magic, but it allows you to check the squares a piece
   can move to without needing to implement rules with loops and conditionals.

Now we've got reasonably fast search, but that's useless if we're no good at evaluating how good a position is.

### Evaluation

The next step is to implement the evaluation function. This is a function that takes a position and returns a score
for that position. The score is a measure of how good the position is for the player to move. The higher the score,
the better the position is for the player to move. The lower the score, the worse the position is for the player to move.
The evaluation function is a huge topic, and there are many different ways to implement it. I won't go into
too much detail, but here are a few of the techniques I implemented:
 - **Piece Values**: This is a simple technique that assigns a value to each piece. The values are based on the
   relative strength of the pieces. For example, a queen is worth more than a pawn.
 - **Positional Values**: This is a technique that assigns a value to each square on the board. The values are
   based on the relative strength of the squares. For example, the center squares are worth more than the edge
   squares for certain types of pieces.
 - **Mobility**: This is a technique that measures how many legal moves a piece can make. The more legal moves
   a piece can make, the better the position is for the player to move.

I didn't spend too much time on this to be honest, and there is a _lot_ of room for improvement.

## Putting It All Together

There's a couple final things we throw in to the final implementation:
 * A book of opening moves. This is a list of moves that are known to be good for the player to move. The
   book is used to quickly find good moves in the opening phase of the game, because it's pretty difficult
   to evaluate the position in the opening phase of the game.
 * A UCI interface. This is a simple interface that allows you to play against the engine using other clients,
   and is how engines like Stockfish and Lc0 expose their functionality.

## Conclusion

This engine is very very simple in comparison to proper chess engines, but it's good enough to beat me easily.
I end up [re-implementing this engine in Rust](/projects/weechess-rs.html), and make some further improvements
to the search and evaluation functions.
