---
name: blossim
github: blossim
description: Slime simulation and animation
layout: project
---

![Screenshot of Blossim](/assets/screenshots/blossim-01.webp)

## About

Fresh in my mind from [building a simple raytracer](/projects/raybaby), I wanted to
keep experimenting with graphics programming using [wgpu](https://wgpu.rs/). On my
list of ideas was a simulation of slime mold. Essentially, you simulate millions
of tiny particles that move around according to some simple rules and render the
results.

My simulation is implemented as a compute shader that runs on the GPU. A simple
render pass then takes the results of the simulation and renders them to the screen.
There's only a few rules that govern the movement of the particles:
- **Diffusion** - Particles emit a pheromone that diffuses through the environment.
- **Attraction** - Particles are attracted to pheromones, and move towards them.
- **Repulsion** - Particles are repelled by other particles, preventing them from
  clumping together.
- **Randomness** - Particles move randomly, but with a bias towards pheromones.

Using these rules, the particles will form a network of connections that resembles
slime mold. The simulation is quite fast, and can handle millions of particles
without any issues.

As always, there is a lot more that can be done to improve the simulation. For example:
 - **More complex pheromone interactions** - We could add more complex interactions
   between pheromones, such as decay or different types of pheromones.
 - **Different particle types** - We could add different types of particles that
   interact with the pheromones in different ways.
 - **Visual effects** - We could add visual effects to the particles, such as
   trails or glowing particles.
 - **User interaction** - We could add user interaction to the simulation, such as
   allowing the user to place pheromones or particles in the environment.

I also had ideas for programming the simulation somehow, by compiling a lisp DSL
into the compute shader. I hope to explore this idea in the future. 
