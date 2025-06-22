---
name: popdoom
github: popdoom
description: A pop-art style renderer for DOOM.
layout: project
---

![Screenshot of Popdoom](/assets/screenshots/popdoom-01.mp4)

_Note: This looks a lot better live without the compression artifacts and at higher resolution._

## About

In 1997, John Carmack released the source code for DOOM for non-profit use. Since then, it's been ported
to any software platform you can think of. In fact, there is a modified version of the DOOM source _specifically_
designed to be ported to new platforms called [doomgeneric](https://github.com/ozkl/doomgeneric).

Ever since I learned about this, I wanted to mess around with it, and recently, I finally had an idea
that seemed worth pursuing: what would it look like to render DOOM in a pop-art style (because, why not)?

## The Implementation

There are several implementations of DOOM that would allow you to change textures or write shaders,
but that would skip the fun of binding code directly to the original DOOM source. So instead of doing that,
we're going to write a custom renderer in Rust and link it against a DOOM library that we'll build.

### Building libdoom

The first step is to build DOOM as a static library. With the right compiler flags and a few tweaks, this
basically works out of the box with `doomgeneric`. The [cc](https://docs.rs/cc/latest/cc/) crate takes
care of compiling the C code and linking it into a static library that we can use in our final Rust binary.

### Wrapping libdoom in Rust

Since we're going to be using the C library from Rust, we need to write some bindings. These are pretty
easy to write referencing the `doomgeneric` headers, and we'll slap a safe Rust API on top of it.

### Running the DOOM Game Loop

DOOM is pretty old. It's no surprise that it has a game loop that is not super compatible with modern
application event loops. Because I chose to build a GPU-accelerated renderer but DOOM uses a software renderer,
I can't just arbitrarily copy the DOOM framebuffer to the screen whenever DOOM tells me to. Instead, we'll
need to juggle the DOOM game loop and our application event loop separately, and shuttle events back and forth
between them.

To do this, we'll run the DOOM game loop completely in a background thread. We'll use buffered `mpsc`
channels to shuttle events back and forth between the two threads. The main thread will pass input events
and control events to the DOOM game loop, and the DOOM game loop will pass framebuffers and other game
events back to the main application event loop. When the main thread receives a framebuffer, it copies the
texture data to the GPU to be used as an input to the graphics pipeline next time we render a frame.

This turned out to work surprisingly well!

### Rendering DOOM

We've now got a regular application event loop and the DOOM framebuffer in our hands. The next step will
be to copy the framebuffer to the GPU and blit it to the screen render target. I've set this pipeline up
a few times before from scratch using WGPU, but this time I'll be using a small crate called
[pixels](https://docs.rs/pixels/latest/pixels/) that takes care of the boilerplate for us. Once
we initialize a pixels instance, we'll get access to the internal WGPU primitives to build a custom
render pipeline that will take the DOOM framebuffer as an input texture and render it to the screen.
 
Hooking this up into our application event loop is straightforward, because this is exactly how it
was designed to be used. Bingo-bango, we have the DOOM framebuffer rendered to the screen!

## Rendering DOOM in Pop-Art Style

Oh man, this turned out to be kinda tricky. I was concerned that DOOM would be too dark and gritty
to be rendered in a pop-art style, but I figured the best way to find out is to try it out. There are
some good resources online about Ben-Day dots and pop-art style rendering, as well as a couple of simple
implementations available on [Shadertoy](https://www.shadertoy.com/). After banging my head against 
the wall trying to port some of the reference GLSL implementations in WGSL, tuning the parameters,
and experimenting with different blending modes, I finally got something working. I think we can all
agree that this doesn't look amazing. It looks okay-ish, and some elements of pop-art come through,
but it's not quite right and stock DOOM is not the best fit for this style.

There are a couple of things I can think of that may improve this:
 * Render DOOM at a higher resolution. This will help avoid smearing the individual pixels and dots
   together to create cleaner shading bands.
 * Use a different color palette. The DOOM palette is pretty dark and gritty, so it doesn't lend itself
   well to pop-art style rendering. A brighter, more saturated palette would probably work better. It's
   possible we could do this by remapping the DOOM palette to a pop-art palette, or by using a
   different texture in the DOOM WAD files.
 * Improve the pop-art rendering algorithm. The current implementation is pretty basic, and I think
   including some more advanced techniques like dithering and noise could help create a more
   interesting effect.
