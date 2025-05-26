---
name: kit-tastrophe
github: kit-tastrophe
source_hidden: true
description: Arcade-style vehicle physics
layout: project
---

![Game screenshot of kit-tastrophe](/assets/screenshots/kit-tastrophe-01.webp)

## About

At any given moment, I've got three or four game ideas rattling around in my head.
I never get past the prototyping phase, often because the idea doesn't end up
feeling novel or fun. But I had one idea with arcade-style vehicle physics that's
been more promising than the others.

I'm not going to give away my precious game idea for free here, but I will talk
about the physics a little bit.

## Vehicle Physics

There's a few different ways to simulate vehicle physics:
 - **Physics/joint-based** - Slap some wheel joints on a box collider and let the
 physics engine do the work
 - **Asset-based** - Use whatever asset your engine provides for a vehicle or download
 one from an asset store, never having to worry about it yourself
 - **Raycasting** - Use a single collider for the body of the car, and use raycasts
 and math to apply physical forces to that body

I built the raycasting solution, and the math that makes it work is pretty amazing
(I did not invent this).

The basic idea is to apply forces to the body of the vehicle based on where the anchor
point of each wheel would be on the body of the vehicle. This way, you can have any
number of wheels, the vehicle responds to physics events in a way that is compatible
with the physics engine (collisions, etc), and you can tune how the vehicle behaves
and feels by changing the parameters of the forces applied, and to which wheels
they are applied.

The setup is simple: you create a rigidbody for the body of the vehicle, and 
identify anchor points for the wheels. Each wheel has a forward vector and an
upwards vector relative to the body of the vehicle that will determine how the
forces are applied to the rigidbody. At each physics step, you calculate and
apply forces for each wheel at its anchor point if that wheel is touching the ground:
 - **Torque** - The force that makes the vehicle go vroom. This force is
 applied in the forward direction of the wheel. This force is evaluated on a power
 curve in order to simulate the engine's power band (ie. you get different power
 at different velocities).
 - **Suspension** - The force is applied upwards from wheel to the body, which
 keeps the vehicle body off the ground. With some dampening, the force behaves
 springy, and the vehicle bounces up and down when it hits bumps or lands after
 a jump.
 - **Steering Friction** - The force that keeps the vehicle from sliding around. This is
 applied in the direction of the cross product of the wheel's forward vector and
 the wheel's upwards vector (ie. in the direction of the axle). It basically
 tries to keep the wheel from sliding sideways, and is also evaluated on some
 curve.
 - **Rolling Friction** - The force that makes the vehicle slow down. This is applied
 opposite to the direction of torque.

Controlling the vehicle is simple: to accelerate, you apply torque to some or all
of the wheels. To steer, you just rotate a wheel's forward vector around its
upwards vector.

With these simple forces, you can tune the vehicle to behave any way that you want:
 * Want some drift? Decrease the steering friction in the front wheels and increase
 torque in the rear wheels
 * Want a heavy vehicle? Increase the mass of the vehicle
 * Want a bouncy vehicle? Increase the suspension force and decrease the dampening
 * Want a fast vehicle? Increase the torque and decrease the rolling friction
 * Want a grippy vehicle? Increase the steering friction
 * Want a vehicle that can jump and land? Apply some additional forces to level the vehicle
   out when no wheels are touching the ground.
 * Want a vehicle that is also a plane? Stop applying the wheel forces in plane-mode.

This results in a vehicle that is very responsive to other physics forces as well. Things
that fall on the vehicle will cause it to bounce on its suspension. You can lift the center
of gravity of the vehicle to make it top-heavy, or put it on 3 wheels and it will topple
over in sharp corners.

If you're interested in more details on the math behind these forces, I recommend checking
out [this video](https://www.youtube.com/watch?v=CdPYlj5uZeI).

The gameplay mechanics to this game are still mostly missing, but just driving these vehicles
around is already a ton of fun. Maybe I'll actually finish this one someday!
