# RGB Life

Conway's Game of Life, played on the pixels of an image, with several species competing for the same
board.

**[▶ Open it](rgb-life.html)** — or download `index.html` and double-click it. No install, no server.

## The idea

Load a photo. Every pixel is asked two separate questions:

- **Is it alive?** Decided by colourfulness and brightness, so blacks, whites and greys become empty
  space.
- **Which species?** Decided by hue — the strongest of red/green/blue, the nearest of six hues, or
  the nearest colour in a palette clustered out of the photo itself.

Then Conway's four rules run on the result: fewer than two neighbours dies, more than three dies,
two or three survives, exactly three fills an empty cell. A recognisable image collapses into
gliders, oscillators and drifting debris within a few generations.

## Species

3 (red/green/blue) or 6 (red/yellow/green/cyan/blue/magenta). Set **Species colours** to *sampled
from the image* and a k-means++ pass clusters the living pixels into exactly that many colours, so
the species become your photo's own palette. Centroids are sorted by hue, which keeps the
wheel-based rules meaningful.

Colour depth (1/2/4/8-bit) posterises the picture *before* that judgement. It changes how the seed
looks; it does not change how many species exist.

## Cross-species interaction models

Within a species the rules never change. What varies is what *foreign* neighbours do to you.

| Model | Cross-species rule | Produces |
|---|---|---|
| **Blind** | a neighbour is a neighbour; birth takes the majority parent | plain Conway — colour is cosmetic |
| **Kin only** | foreigners are invisible; only kin count | parallel populations sharing one grid |
| **Tension** | effective count = friends − enemies | fronts annihilate; permanent no-man's-land |
| **Kin crowding** | solitude judged by kin, crowding by everyone | mixing itself is lethal |
| **Mutualism** | a cell that would die alone survives if a foreigner touches it | interlocking two-tone tissue |
| **Hybrid birth** | mixed parents produce the hue between them | new colours appear only at borders |
| **Keystone predator** | one chosen species hunts all the others | one colour polices the board |
| **Safety in numbers** | the same hunt, but only isolated prey is taken | predation carves edges, not cores |
| **Mimicry** | the outnumbered convert to the majority instead of dying | fronts advance by conversion |

Contested births — where the parents disagree — resolve by random, round robin, rarer-wins,
commoner-wins, hue blend, mutation, lowest-species, or nobody.

## Pattern library

Nineteen patterns across still lifes, oscillators, spaceships, methuselahs and the Gosper glider
gun, each with a preview, rotation and flipping. **Every one was verified against this simulator
before being included** — Diehard vanishes at generation 130, the glider translates one cell
diagonally every four generations, and two of the original spaceship encodings were wrong and got
corrected.

You can also drag a rectangle around any region of the board and save it as a reusable stamp, with
its per-cell colours preserved. Saved clips export to JSON.

## Controls

| | |
|---|---|
| **Space** | play / pause |
| **←** **→** or **,** **.** | step a generation |
| **0–6** | pick the brush species; 0 or **E** erases |
| **[** **]** | brush size — hold shift for jumps of five |
| **S** / **P** / **L** | select area / pattern library / load image |
| **F** / **Shift+F** | fit to window / one cell per pixel |
| **H** **G** **T** | pan / grid / motion trails |
| **R** **C** | random soup / clear |
| **⌘/Ctrl+Z** | undo — add shift to redo |
| **I** **Esc** | info panel / cancel whatever is armed |

With a stamp armed, **R** rotates and **F** flips it instead.

## Performance

Boards run up to about 6 million cells. Neighbour counting uses a sliding column-sum per scanline
rather than gathering eight cells per site, which is roughly twice as fast and produces bit-identical
output; rendering writes 32-bit words rather than four bytes per pixel.

| Board | Cells | Speed |
|---|---|---|
| 512×384 | 197k | ~110 gen/s |
| 1080×720 | 778k | ~26 gen/s |
| 1920×1080 | 2.07M | ~10 gen/s |

Step-back history is budgeted by memory, not frame count — 400 generations on a small board, about
24 at full HD.

## Things worth trying

- Six species sampled from a sunset photo, **Mimicry**, motion trails on.
- **Keystone predator** with prey converting: watch one colour eat the picture.
- A blank board, **Hybrid birth**, and a few stamped gliders aimed at each other — the collisions
  produce colours that were never in the seed.
- Zoom to 8× on one corner and run at 120 gen/s.
