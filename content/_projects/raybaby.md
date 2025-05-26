---
name: raybaby
github: raybaby
description: Baby's first raytracer
layout: project
---

![Screenshot of a render from raybaby](/assets/screenshots/raybaby-01.webp)

## About

I had recently heard about [wgpu](https://wgpu.rs/), a cross-platform graphics API, 
and wanted to toy around with it. I've had some experience using OpenGL, but always
greatly disliked the API. I also had previous experience with Vulkan, but its verbose
design and complexity makes it difficult for experiments, where the architecture
of the application evolves rapidly. Wgpu sits nicely in the middle, and being a
Rust library, I expected the semantics of the API to be both more ergonomic and
more obvious, thus helping me learn the API faster (I found this to be true).

## The Triangle

So the obvious thing to do is get that hello-world graphics triangle rendering on screen. 
There's a bit of boilerplate to set up the window, create a device, swapchain, buffers,
queues, etc, but the docs are pretty good and copy-pasting will get you a triangle.

## The Raytracer

As it turns out, a bad raytracer isn't that complicated. Take a vector pointing from the 
current pixel through the camera frustum, and check if it intersects with any of the
objects in the scene. If it does, tint the pixel with the color of the object, determine
a bounce direction, and repeat until you hit a light source or the maximum number of bounces.

There's a bunch of fiddly linear algebra to get right and it's not super fun debugging
all this math in shader code, but it's pretty cool to see your scene come to life for
the first time!

Building on this foundation you can add new geometry types, ambient lighting, focal blur,
and so on. I also added an immediate-mode GUI to control the camera, a declarative
DSL for defining the scene, and a few other niceties. To my eyes, the renders actually
look pretty good! They take a while though, because we've put almost no effort into
optimizing this thing.

## The Future

There's a lot of room for improvement here. This implementation barely supports
meshes, and to do so would require specific spatial partitioning optimizations.
We could add support for textures, more types of lights, and much more. Still, 
pretty good for ~300 lines of WGSL eh?
