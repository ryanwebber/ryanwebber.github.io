---
name: wordboy
github: wordboy
description: Wordle on the GameBoy Advance
layout: project
---

![Screenshot of Wordboy](/assets/screenshots/wordboy-01.webp)

## About

I thought it would be interesting to try and make a GameBoy game. This would
give me an opportunity to learn more about the quirks of the GBA hardware
and a reason to give no-std rust a try. It's built from scratch using only
bitfiddling dependencies.

The game is a clone of Wordle with a simplified dictionary for ROM saving reasons (a
better compression scheme for the dictionary would likely allow for a much better
experience).

![Screenshot of Wordboy](/assets/screenshots/wordboy-02.webp)

Without getting too in the weeds, the game uses tile-mode graphics, 8 bits per pixel,
and 16x16 tile sizes. Most of the work was just learning about the architecture,
defining memory mapping in Rust, and building a small build pipeline to compile
bitmaps into tile data and object data for use at runtime.

## Download

The ROM is available for download on the [github page](https://github.com/ryanwebber/wordboy) as a release artifact. Give it a
try in any GBA emulator that doesn't require a valid checksum header.
