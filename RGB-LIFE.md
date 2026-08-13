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

## Rules, in four independent parts

The thresholds never move — under two neighbours is solitude, over three is crowding, two or three
survives, three fills a cell. What you choose is which neighbours those counts are taken over:

**Death — what kills a living cell**

| | |
|---|---|
| Any neighbour counts | plain Conway; colour is cosmetic |
| Kin only | foreigners are invisible — parallel populations on one grid |
| Kin for company, anyone for crowding | mixing itself is lethal |
| Friends minus enemies | fronts annihilate; a permanent no-man's-land opens |
| Mutualism | a cell that would die alone survives if a foreigner touches it |
| Hunted | one species preys on all the rest |
| Hunted unless sheltered | the same hunt, but two kin beside you is shelter |

**Birth — when an empty cell fills:** any three neighbours · three of one species · three left after
enemies cancel friends.

**Newborn colour:** majority parent · blend of their hues · a random parent weighted by count · round
robin · the rarer species · the commoner species · mutation · lowest · nobody.

**Allegiance — whether a living cell can change species:** never · conquest (adopt whoever dominates)
· mimicry (defect to the biggest rival whenever outnumbered, including standoffs where nothing
dominates outright).

A **Preset** menu fills all four at once for the named combinations — Classic Conway, Kin only,
Tension, Kin crowding, Mutualism, Hybrid birth, Keystone predator, Safety in numbers, Mimicry — and
flips to Custom the moment you change one.

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

- Six species sampled from a sunset photo, the **Mimicry** preset, motion trails on.
- **Keystone predator** with prey converting: watch one colour eat the picture.
- Death set to *mutualism* with newborn colour set to *blend* — a combination none of the presets covers.
- A blank board, **Hybrid birth**, and a few stamped gliders aimed at each other — the collisions
  produce colours that were never in the seed.
- Zoom to 8× on one corner and run at 120 gen/s.
