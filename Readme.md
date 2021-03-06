synthesis.js
=============

<img src="./examples/images/trees.png"  height="150"></img><img src="./examples/images/CDLOD.png" height="150"></img>

Overview
--------

A content synthesis library for [WebGL](http://en.wikipedia.org/wiki/WebGL)
and [three.js](http://threejs.org/).

Core:

* Randomness - random distribution utilities (uniform, gaussian, power law)
* Consistent Noise - consistent noise algorithms (simplex noise, worley noise)
* L-Systems - self-similar structures, e.g. trees, plants
* Marching Cubes - produces meshes from scalar fields, e.g. caves, coral reefs
* Color - color palette generator (complementary, split complementary, etc..)

Rendering:

* Heightmap Level-of-Detail Rendering - CDLOD implementation for rendering large heightmaps.

Application libraries:

* Trees
* Terrain (Flat, Spherical)
* Water (planned)

Purpose
-------

This project is mainly intended for my own learning and experimentation purposes,  but others are
free to use it.

The main reason I've put it together is to gather a variety of algorithms and techniques
for generating synthetic content into a single place where I can easily combine them together
to test some concepts I am exploring.

There are a lot of elements of procedurally generated content in this project but it is not intended
as a procedurally generated content toolset in the purest meaning of the term.  In particular,  this
project does not contain utilities to help use a a single random seed to deterministically generate a entire world or
universe.  Some of components of this library could be used toward that end,  but they could just as
easily be used to build content combined with artwork created by hand.

Design goals
------------

* Provide consistent APIs to core utilities, written in the OO style of three.js.

* Composition - Should be easy to build up new applications of synthetic content
  by composing the core utilities in this library in new and unique ways.

* Convenient parameterization - allow for some parameters to be constrained to ranges, distributions
  or exact values, with reasonable defaults for unconstrained parameters

* Visual Appeal - Coordinated colors and modern graphics lighting and camera techniques should
  be enabled by default.


Core utilities
==============

Randomness
----------

<img src="./examples/images/uniform3d.png" width="80"></img><img src="./examples/images/normal3d.png" width="80"></img><img src="./examples/images/pareto3d.png" width="80"></img>

Consistent Noise
----------------

<img src="./examples/images/simplex.png" width="80"></img><img src="./examples/images/worley1.png" width="80"></img><img src="./examples/images/worley2.png" width="80"></img><img src="./examples/images/worley-coloring.png" width="80"></img>

Simplex provides a consistent noise source, which can be used to generate random terrain height maps
or surface textures.

Worley provides consistent noise which can be used to represent stone textures, cellular structure,
ridge lines, etc...

For all consistent noise algorithms, results are deterministic.  To add some randomness, one may pick a
random starting coordinate before sampling.

L-Systems
--------

<img src="./examples/images/trees.png" width="400"></img>

Generates random trees using a stochastic L-System and renders them using
WebGL and three.js.

Marching Cubes
--------------

<img src="./examples/images/marching-cubes.png" width="400"></img>

Marching cubes can create a mesh from a scalar field.   It has many applications,
and is a powerful tool in the procedural generation toolbox.  It can, for example,
be use to generate complex natural terrain that inclues caves and arches.

Color
-----

Add basic routines, start with: http://blog.noctua-software.com/procedural-colors-for-game.html


Heightmap Level-of-Detail Rendering
-----------------------------------
<img src="./examples/images/CDLOD.png" width="600"></img>

CDLOD can render large heightmaps with high detail
for portions of the heighmap near the camera and
low detail for portions far away from the camera.
Allowing for very large scenes to be efficiently
rendered.  Morphing is used to smoothly transition
level of detail as the camera moves around the scene,  and a 3D distance based detail calculation
make means this approach works well even when
the camera is far from the height map,  such
as far above it.  It could, for example,  be
used to render a planet from space, and support
continuous zoom all the way down to the surface.

TODO
----

* [ ] Clean up shader code.
* [ ] Make separate material for CDLOD.
* [ ] Add atmosphere example
* [ ] Add glsl noise texture generator (2D and 3D)
* [ ] Add turbulence noise from http://www.decarpentier.nl/scape-procedural-extensions

* [ ] Clean up examples.

* [ ] Clean up Color converter to use classes, add a lerp operation to new HCLColor class
* [ ] Update Gradient generator to use HCL, should better for lerp than RGB
* [ ] Add Markov Chains
* [ ] Fill in "holes" in HCL space, see http://blog.noctua-software.com/procedural-colors-for-game.html for details.

* [ ] Fix marching cubes so all parameters can be passed in, have good defaults
* [ ] Add vertex smoothing/removal routine for marching cubes.
* [ ] Generate only a skeleton from l-systems, find a way for them to be skinned in a general way (via vertex shader?)
* [ ] make symbols that can be used in l-systems extensible, so alternatives to the leaf can be added
* [ ] Remove cylinder and sphere shapes (or at least the sphere leaf shape)
      from the l-system code,  introduce a class that can be used in it's place.
      The class should be parametric.
* [ ] non-visual procedural applications, e.g. NPG-AI

Building
========

Setup
-----

Requirements:

* npm
* grunt
* bower

```sh
npm install -g bower
npm install grunt --save-dev
npm install grunt
npm install -g grunt-cli
```

```sh
npm install
bower install
```

Build
-----

```sh
grunt
```

Resulting synthesis.js is written to build/synthesis.js.

Adding Dependencies
===================

For runtime dependencies:

Warning!  Runtime dependencies should be kept to the absolute minimum.  Please
consider alternatives before proposing an additional dependencies.

```sh
bower install <package> --save
```

For build system dependencies:
```sh
npm install <package> --save-dev
```

Modify package.json.  Make the version of the dependency exact by removing the
"^" prepended to the version.  This eliminates potential build consistency
issues.
