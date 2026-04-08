---
name: bitsle
github: bitsle
description: Game engine / editor written in C
layout: project
source_hidden: true
---

![Screenshot of Bitsle](/assets/screenshots/bitsle-01.webp)

## About

There's a saying:

> "Don't roll your own game engine."

So... I've rolled my own game engine! This means I don't intend to actually ship a game, so
why did I do it?

I'm the kind of person that gets immense satisfaction from tweaking APIs and architectures
until they look like something beautiful to me. That's why, despite game engines like Unity
and Godot being massively popular and capable, I've always wanted to build my own that
smooths out some of the quirks and design decisions that stick out to me as awkward. Things
like the `MonoBehaviour` class most scripts inherit from in Unity:
 * What an awful name. Why include the current implementation of the .NET Framework you happen
   to be using in the typename of a public class? Why use the spelling "Behaviour" with "o-u-r"
   here but use the spelling "Color" with "o-r" for other APIs? 
 * The inheritance hierarchy is a bit of a mess. There are magic methods that get called,
   a bespoke initialization process, leaky abstractions, you name it
 * It's full of footguns like APIs that return null pointers, callback ordering issues,
   a confusing lifecycle hierarchy, etc
 
I'm not trying to pick on Unity here, nor did I bother to research why these things are the
way that they are. I'm sure that some of these things are language problems, most of this
stuff is well-documented and doesn't actually impact you that often, and almost all of it is
probably just the result of certain design decisions aging slowly over time, but the point is,
these things irk me a bit. Surely if I made my own game engine, I wouldn't make any of the
same mistakes as teams of people way more experienced and knowledgeable than me, and I could 
make something _beautiful_, right?

## The Engine

So I built one, in C. Why C you ask? Because come on, it's not realistic to think I can actually
build a fully functional, usable, competitive game engine by myself. It's for me, which means
that I can do what I want, even if it's not remotely practical. 

It has:
 * Support for macOS, Linux, and Windows, thanks to Sokol which provides platform-agnostic APIs
   and a graphics backend that supports GLES3/WebGL2, GL3.3, D3D11, Metal, and WebGPU
 * Support for 2D and 3D, support for audio, user input, etc 
 * An actually useful editor, powered by imgui, that does many of the things you'd expect from a
   game engine's editor (scene view, game view, editable components, entity selection, gizmos,
   widgets, scene hierarchy, resource previews, etc)
 * A simple raycasting/overlap testing physics engine (not a full simulation)
 * Opt-in support for a scripting language (Ruby), to build game components in instead of using
   the C API
 * Hot-reload support, by dynamically linking and loading game code as a shared library 
 * An asset bundling and packing system, combined with a virtual filesystem in-game for loading
   resources
 * A scene-based scripting system, modeled after Godot and Unity APIs with some inspiration
   from ECS. A game is made up of scenes, entities, components, and systems. 
 * Decent performance (thanks largely to instancing and cache locality of the scripting system)
 * Fast compilation speeds (< 3 seconds to build the engine. Rebuilding and hot-reloading game
   code is near-instant)
 * Some tests!

That being said, it's questionable if you could really build a game in it yet. I haven't tried.
But more importantly (to me), I _like_ it. I like the architecture. I like the design. I like
the entity/component APIs. I like how insanely fast it is compared to literally anything else
I've ever tried. I like that I can just add features to the game engine if I want them. I like
tweaking the colors of things. It exists only because I like the shape of it.

<br/>

¯\\_(ツ)_/¯
