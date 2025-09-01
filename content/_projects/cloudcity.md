---
name: cloudcity
github: cloudcity
description: A point-cloud renderer for LIDAR survey data
layout: project
---

![Screenshot of CloudCity](/assets/screenshots/cloudcity-01.webp)

## About

You're just going to have to trust me that this is _way_ cooler than the screenshot above
unless you want to download gigabytes of LIDAR data and run the renderer yourself. 

This is an interactive renderer for point-cloud data. Specifically, I built this to
render LIDAR survey data that my home city of Vancouver (Canada) commissioned in
2022 and has made publicly available. The points seem to be clustered about
~1m apart from each other, and the entire downtown core of the city is represented
in a few billion points.

A still image really doesn't do this justice -- in a live environment, the point-cloud
rendering feels like a straight up sci-fi hologram. I tried to capture a screen recording
of this, but compression artifacts really ruin the effect.

## The Tech

This is built using the WGPU graphics APIs. The primary performance optimization that
makes realtime rendering possible is frustum culling implemented in compute shaders,
which massively reduce the number of points actually needed to be rendered. Given the
level of detail in the dataset, the remaining points in actual view of the camera (up
to a certain distance) can all be rendered in a handful of instanced draw calls.

On my humble MacBook Pro, I can run this at a buttery-smooth 120 FPS.

## Enhancements

The main enhancement I want to make is the ability to stream point data to the GPU
as necessary. Currently, this demo is actually limited by memory, not by frame
processing or rendering. With more than around 8 million points, the buffers
exceed the limit for the culling and rendering pipelines.

With point streaming implemented (from disk and to the GPU), it would theoretically
be feasible to render the entire dataset, regardless of its size, at the LoD that
is available.
