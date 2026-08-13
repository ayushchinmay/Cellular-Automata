# Rule X

One row of cells. Eight yes-or-no answers. Everything below follows.

**[▶ Open it](rule-x.html)** — or download `rule-x.html` and double-click it. No install, no server.

## The idea

An elementary cellular automaton is the smallest interesting automaton there is. A single row of
cells, each on or off. To draw the next row, every cell looks at three cells above it — its own and
one either side — and asks what to become.

Three cells, each on or off, is exactly eight situations. Give an answer to each and you have
described the automaton completely. Read the eight answers as binary and you get a number from 0 to
255, and that number **is** the rule. Rule 30 is `00011110`.

Those eight answers are the eight boxes above the diagram. The little pattern on top of each box is
the situation; the square underneath is your answer. Click one and the diagram redraws immediately
under the new rule — the number and the binary code follow along, rather than the other way round.
Keys **1–8** flip them left to right.

## Rule 2X

Switch **Mode** to four cells and the same idea gets bigger: sixteen situations, sixteen answers, and
a rule number from 0 to 65,535.

Each box still carries one answer square rather than two. Sixteen situations with two outputs each
would be 4¹⁶ — about 4.3 billion rules — and a strip nobody could read.

Four cells have no middle one, which creates a real problem: where does the new cell go? The honest
answer is the **gap** between the two centre cells, and that is what **stagger rows** does — every
other row shifts half a cell, so each new cell sits directly under the gap it came from and the
pattern stays centred. Turn it off and the window becomes lopsided, reaching one cell left and two
right, so everything slides sideways. Measured over 200 symmetric rules: half a cell of drift with
stagger on, eight cells without it.

## The seed

**Sources** runs from 0 to 24.

- **1** is the classic — a single cell in the middle, which is where Rule 30 and Rule 110 get their
  reputations.
- **0** leaves the row empty so you can drag along the top of the board and place cells by hand.
- **n** places cells at equal spacing. Each one grows its own cone until the cones meet, and the
  interference is usually more interesting than either tree alone.

## The board

Width is 101, 201, 401, 801 or fit-the-window; cell size runs 1–16 px. **Wrap edges** joins the two
ends into a ring, so a pattern that runs off the right returns on the left.

At the bottom of the board there are two behaviours, and they suit different jobs:

- **Keep going** — the board scrolls and every row is kept (up to 20,000). The wheel or a drag takes
  you back to the seed; **Home** and **End** jump.
- **Stop** — the diagram behaves like a sheet of paper. Height is whatever fits on screen and it
  halts the moment the pattern reaches the last row. This is the one you want before saving a
  picture. Change the cell size or resize the window and the sheet resizes with it.

## Reading the diagram

**Colour cells by** has two settings. *On or off* is the familiar two-tone diagram. *Which answer
fired* tints each cell by the box responsible for it, which turns the picture into a map of which
part of your rule is doing the work — usually a surprise, because most rules lean hard on two or
three of their eight answers.

Six palettes: amber, crimson, jade, cyan, azure, violet.

**Compare with a second rule** runs two rules from the same seed at once. *Overlay* draws both;
*only where they differ* draws just the disagreements, each in the colour of whichever rule turned
that cell on, and the **Match** stat reports what percentage of the current row the two rules agree
on. Rule 110 against 124 (its mirror) or 137 (its on/off swap) is the obvious first experiment.

**Save diagram as PNG** exports exactly what is on the board, named after the rule.

## The rules panel

A catalogue of notable rules, grouped by what they do, each with a preview computed live from a
single centre cell rather than shipped as an image:

| | |
|---|---|
| **Nested** | 90 Sierpiński triangle · 22 triangles within triangles · 150 XOR of all three · 60 one-sided Pascal |
| **Chaotic** | 30 order left, chaos right · 45 chaos, drifting · 105 dense noise · 126 nested, then broken |
| **Complex** | 110 proved universal · 54 gliders on a lattice · 124 110 mirrored · 137 110, on and off swapped |
| **Traffic** | 184 cars moving right · 226 cars moving left |
| **Simple** | 1 checkerboard · 255 everything fills |

In Rule 2X mode there is nothing to catalogue — 65,536 rules is far too many — so the panel shows a
random dozen with a **Reroll** button.

## Stats

Row number, live cell count, density as a percentage, the eight- or sixteen-bit code, and match
percentage when comparing. The strip beside them is a density sparkline over the last 600 rows: for
Rule 30 it settles into noise around a stable mean, for Rule 110 it stays visibly structured, and
for a nested rule it oscillates on a clean period.

## Why any of it matters

Rule 30 turns one black square into something disordered enough that its centre column passed serious
randomness tests, and Wolfram used it as a random number generator in Mathematica. Rule 110 was
proved capable of universal computation — it can, in principle, run any program. Both are eight
yes-or-no answers applied to one row of cells.

## Controls

| | |
|---|---|
| **Space** | play / pause |
| **←** **→** | step a row |
| **[** **]** | previous / next rule |
| **1–8** | flip that answer box (Rule X only) |
| **R** | random rule |
| **S** / **C** | single centre source / empty seed |
| **0** | reset to the seed row |
| **F** | fit width to the window |
| **Home** **End** | top and bottom of the diagram |
| **PgUp** **PgDn** | scroll a screenful |
| **X** | compare on / off |
| **I** **Esc** | info panel / close |

Top bar has **Reset**, **Fill** (run until the board is full), row transport with **+10**, and speed
from 1 to 120 rows/s. Drag on the top row to draw the seed by hand.

## Things worth trying

- Rule 30 with **which answer fired** on — the chaos resolves into a small number of repeating
  decisions.
- Rule 90 with **sources** at 4, wrap on. Four Sierpiński triangles colliding on a ring.
- Rule 110 versus 124 in **only where they differ**, and watch the match percentage.
- Rule 184 with a dense hand-drawn seed. It is a traffic model: cells are cars, and jams move
  backwards while cars move forwards.
- Rule 2X mode, stagger on, **Reroll** the sampled gallery a few times. Sixteen-bit rule space is
  almost entirely unexplored — most of what you see there has probably never been looked at.
