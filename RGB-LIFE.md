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

3 (red/green/blue) or 6 (red/yellow/green/cyan/blue/magenta), assigned by fixed hue sectors — a
pixel's hue decides its side. **Palette** under Display changes how those species are drawn (Signal,
Pure RGB, Bloom) without changing who belongs to whom.

Colour depth (1/2/4/8-bit) posterises the picture *before* that judgement. It changes how the seed
looks; it does not change how many species exist.

## Rules, in two parts

Conway's counts — under two neighbours is solitude, over three is crowding, two or three survives,
three fills a cell — are the starting point, not a fixed law. You choose an **interaction**, which
decides whose presence counts, and the **neighbour counts**, which decide how many it takes:

**Interaction — whose presence counts**

| | |
|---|---|
| Classic Conway | colour is cosmetic; newborns take the majority |
| Kin only | foreigners are invisible — parallel populations on one grid |
| Tension | friends minus enemies; fronts annihilate and a no-man's-land opens |
| Kin crowding | kin for company, anyone for crowding — mixing itself is lethal |
| Keystone predator | one species hunts all the rest, with a threshold and optional conversion |
| Hybrid birth | classic counts, but newborns take the hue *between* their parents |

**Neighbour counts — how many it takes.** Nine toggles for birth and nine for survival, plus named
families: Conway B3/S23, HighLife B36/S23 (which has a replicator), Day & Night, 2×2, Maze
B3/S12345, Mazectric, 34 Life, Seeds B2/S. Set **These counts apply to** to *each species
separately* and one colour can run Maze while another stays Conway on the same board — pick the
species you are editing from the swatches. A birth count of 0 never fires: an empty cell with no
neighbours has no species to inherit.

The counts are independent of the interaction and apply on top of whichever one is chosen.

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

- Six species sampled from a sunset photo, **Keystone predator**, motion trails on.
- **Keystone predator** with prey converting: watch one colour eat the picture.
- Death set to *mutualism* with newborn colour set to *blend* — a combination none of the presets covers.
- A blank board, **Hybrid birth**, and a few stamped gliders aimed at each other — the collisions
  produce colours that were never in the seed.
- Zoom to 8× on one corner and run at 120 gen/s.
- Per-species counts with **Kin only**: give one species B3/S23 and another B3/S1234, then watch the
  second one strangle the first purely by outliving it.
