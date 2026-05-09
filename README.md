# Verification of Rubik's Cube Solving Techniques Used by Humans
The CFOP/Fridrich method of speedsolving is a very popular technique
for solving Rubik's cubes quickly (hense, "speed solving").

For laypeople who don't know how to solve a Rubik's cube, it may seem
very daunting to actually solve a cube.

Speedsolving methods break down solving the cube into several stages
that build on each other, which forms a kind of abstraction/reduction
of manipulating the cube to only a small section of the cube. Each
step's moves have an invariant that is preserved.

CFOP stands for Cross-F2L-OLL-PLL, which in turn stands for
- Cross: You make a cross in the correct orientation on one side of a
  cube. All of the edge piece stickers connected to that side (e.g.,
  adjacent sides to the solved cross one) need to match with the
  center pieces. Sequences of moves for solving the cross need to not
  mess with already solved parts of the cross. This ensures that you
  constantly make progress.
- F2L: First two layers. This step involves solving the two layers
  that are closest to the solved cross. Sequences of moves used here
  may permute the cubies (the individual little "cubes" that make up
  the faces of the Rubik's cube) in the last layer and the first two
  layers in a given corner, leaving the other corners of the first two
  layers alone.
- OLL: Orientation of the last layer. We finally care about the last
  layer, but only insofar as whether the last remaining face looks
  complete. The edge and corner cubies may not be the correct colors,
  but the goal is to get all the cubies in the last layer to have the
  same color on top (the face opposite from the face you did the cross
  on). This orientation is an invariant in the next step.
- PLL: Permutation of the last layer. This step moves around cubies in
  the last layer without altering the orientation.

Some things that I like about this (i.e., connections to software
engineering, PL, and math):
- when you're working on the cross, you can consider all the states of
  the cubies equivalent up to where the edge cubies that make the
  cross are. (If you're actually competitive in speedsolving, you
  probably want to try to combine some of the cross and F2L, but
  that's breaking an abstraction barrier.)
- other steps similarly ignore other parts

We can have some sort of very basic transition system between these
steps, and prove various invariants about the transition system such
that eventually, it arrives at a solved cube.
