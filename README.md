# bandaged-cube-explorer
## Introduction
A bandaged cube is a 3x3 Rubik’s cube, where several sets of cubies have been fused together to form larger solid blocks (pic. 1). These blocks will prevent various faces from being turned at various times; as a result, bandaged puzzles can be much harder to solve than non-blocking ones. A bandaged cube is a type of a [twisty puzzle](http://www.google.com/images?q=twisty+puzzles).

<p align="center">
  <img src="https://ladislavdubravsky.files.wordpress.com/2015/03/justinpocket2.png">
  <br> Picture 1 - a bandaged cube, solved state
</p>

## Mathematical model
A bandaged cube can be modelled as a [groupoid](https://en.wikipedia.org/wiki/Groupoid), i.e. a category, where every morphism is invertible. Objects for the groupoid are the distinct states of the puzzle after it was drained of color (see pic. 2). We call these objects bandage shapes. Morphisms are generated by single face turns transforming one bandage shape to another (but not [freely generated](https://ncatlab.org/nlab/show/free+groupoid), as they're subject to relations arising from their [representability](https://ncatlab.org/nlab/show/representation) as permutations of Rubik's cube's 54 stickers). Standard Rubik’s cube has only one bandage shape and its groupoid simplifies to a group.

<p align="center">
  <img src="https://ladislavdubravsky.files.wordpress.com/2015/03/justinpocket4.png">
  <br> Picture 2 - the solved bandage shape for the Picture 1 cube
</p>

Sequences of single physical face turns on the puzzle cannot in general be composed. However, those turn sequences that both start and end in a same bandage shape can always be composed. Set of all such turn sequences together with their composition forms a group, the isotropy group of the given bandage shape. It can be shown that any two bandage shape isotropy groups are isomorphic, therefore we can talk about a well-defined group for a given bandaged cube.

Bandage shape objects together with the generating morphisms form a graph, the bandaged cube graph (pic. 3). One approach to the solution process for a bandaged cube is as follows.

<p align="center">
  <img src="https://raw.githubusercontent.com/ladislavdubravsky/bandaged-cube-explorer/master/pics/bcfuse.png">
  <br> Picture 3 - the bandaged cube graph of some bandaged cube
</p>

1. Find for any bandage shape a turn sequence to transform it into the solved bandage shape *S*. While the bandage shape is now correct, it's likely that the colored stickers don't yet form monochromatic faces, as same dimension blocks can often be permuted and 1x1x1 cubies also flipped or twisted (pic. 4).
2. Gather enough useful loops *{L<sub>1</sub>, ..., L<sub>n</sub>} = L* from shape *S* to itself to generate the full isotropy group of *S*.
3. Use techniques for factorization in groups to decompose the remaining colored stickers permutation into a product of turn sequences from *L*.

<p align="center">
  <img src="https://ladislavdubravsky.files.wordpress.com/2015/07/sr_block.jpg">
  <br> Picture 4 - a bandaged cube, that is in its solved bandage shape, but not solved
</p>

Python code in this repository focuses on the graph-navigating part of the solution process. In human solving, the enjoyability follows from being able to form at most a small set of rules for proceeding forward, using visuospatial recognition and imagination and ad-hoc logic. Harder bandaged puzzles don’t seem straightforwardly amenable to such approach though.

## Purpose of this project
To provide an exploration and prototyping platform for understanding bandaged 3x3 cubes. Aid solvers in their endeavors and help me write my [blogs](https://ladislavdubravsky.wordpress.com/).

## Usage
#### General
You need to have Python installed together with packages such as numpy, matplotlib or networkx - these come bundled with distributions such as WinPython. After that you need to place the project files somewhere the interpreter can see them, for example to a location on your PYTHONPATH environment variable.

#### Bandage shape model in code
Until we’re in need of efficiency increase, we represent a bandage shape as a list of length 27 – a cubelist. This is a 0..26-indexed list, and ternary representations of its indices correspond to coordinate triplets for unit cubies in a 3x3x3 cube (even if there is no central cubie in the physical cube). The list elements are numbers coming from range 1..(number of bandage shape blocks). Cubelist indices containing an identic cubelist element represent a single bandaged block. It follows the indices should form a geometric cuboid in their ternary representation. For a unique bandage shape representation, we also demand of cubelist elements to be increasing in the order of their first occurrence in the cubelist reading order (this gets taken care of under the hood).

For convenience, however, zeros can also be used in a cubelist. Any zero represents an isolated 1x1x1 cubie. Thus you can input e.g. the standard Rubik’s cube as ```[0]*27``` instead of its proper representation as ```list(range(1, 28))``` (to which it’ll get normalized under the hood).

#### Example script usage.py
Follow the usage.py script to see some examples of existing functionality use.
