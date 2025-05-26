---
name: neyowm
github: neyowm
description: A basic autopilot controller for single-engine aircraft in X-Plane 11
layout: project
---

![Screenshot of Neyowm](/assets/screenshots/neyowm-01.webp)

## About

Somehow I learned that NASA maintains an X-Plane plugin called
[XPlaneConnect](https://github.com/nasa/XPlaneConnect) that exposes an XPC interface
for X-Plane 11 over UDP. Wait... does that mean I can write my own autopilot? Yes, yes it does.

I built a small Rust library to wrap the C API of the plugin, hacked together an in-process
pub-sub architecture for the autopilot subsystem, and slapped a simple terminal UI on top of it.

The autopilot is a simple PID controller that can be used to control the aircraft's
heading, altitude, and speed. It currently only supports single-engine aircraft,
and is only able to maintain a constant altitude and heading. But hey, it won't crash
the plane (usually)!

## PID Controllers

What I found most interesting about this project were the
[PID controllers](https://en.wikipedia.org/wiki/Proportional–integral–derivative_controller).
I've never really crossed paths with these before, despite being faced with problems in the past
that could have been solved with them (especially in game development). These nifty little controllers
are just a bit math that attempts to minimize the error between a desired value and the current
value over time. For example, if you want to maintain an air speed of 200 knots, and have access
to a speedometer and throttle, you can use a PID controller to adjust the throttle to converge on
and maintain that speed. 

Specifically, the PID controller uses three terms:
- **Proportional**: This term is proportional to the error between the desired value and the current
  value. It provides a correction based on how far off you are.
- **Integral**: This term is proportional to the accumulated error over time. It provides a correction
  based on how long you've been off target.
- **Derivative**: This term is proportional to the rate of change of the error. It provides a
  correction based on how quickly you're approaching the target.

By tuning the coefficients of these three terms, you can adjust how aggressively the controller responds
to changes in the error. Tuned correctly, the controller will quickly converge to the desired value
without oscillating or overshooting. Tuned poorly, the controller will oscillate or overshoot
the target, or even diverge completely. It can be tricky to get the tuning right, but there are
some techniques to help with this.

Now that I know these PID hammers exist, I'm seeing a lot of PID-shaped nails everywhere...
