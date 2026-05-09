# Verification of Rubik's Cube Solving Techniques Used by Humans
The CFOP/Fridrich method of speedsolving is a very popular technique
for solving Rubik's cubes quickly (hence, "speed solving").

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
- moves that don't change the orientation of a cubie 

We can have some sort of very basic transition system between these
steps, and prove various invariants about the transition system such
that eventually, it arrives at a solved cube. 

The specification of every algorithm (sequence of moves to solve a
particular case) looks something like this:
```lean
{ c : Cube // P c /\ unsolved c_i } -> { c : Cube // P c /\ solved c_i }
```
(This actually reminds me of separation logic, mostly in that you know
that different parts are disjoint, and `P` cannot be too strong a
statement about `c` if `P c` is still going to be true afterwards.)

Eventually, we come to a point where we have
```lean
(fun c => solved c_0 /\ ... /\ solved c_n) c
```
and at that point, the entire cube is solved.

The specification of the OLL and PLL algorithms however is a bit
different. OLL algorithms look something like this:
```lean
{ c : Cube // P c /\ ~oriented c_i } -> { c : Cube // P c /\ oriented c_i }
```
Then PLL algorithms look like this:
```lean
{ c : Cube // P c /\ oriented c_i /\ ~solved c_i} ->
{ c : Cube // P c /\ oriented c_i /\ solved c_i
```



## Musings
The cross ends up being a lot of cases that we think are boring, so we
only start from states that have already instantiated the
cross. (Human solvers usually learn how to intuitively solve the
cross).

We will actually instantiate the beginner's method for the first 2
layers, since there are less cases for each of the first 2 layers and
it is more mechanical.

The cases for the F2L as presented on
[solvethecube.com](https://solvethecube.com/algorithms#f2l) are not
exhaustive. For several of them, the website actually presents them 
in their canonical form. Most of them can be transformed into their
canonical form through U rotations. However, there are cases of
section 5 (corner in bottom, edge on top) where the edge in question
is currently trapped in the middle and on a different edge than your
target edge. These require that you use some combination of 1 of F/R/L/B
and some applications of U to get to the canonical form.

From F2L, "1 look OLL/PLL" (being able to solve OLL in 1 "step"
(application of a memorized set of moves) to reach PLL, and likewise
with PLL to reach a solved state) becomes a bunch of special cases.

