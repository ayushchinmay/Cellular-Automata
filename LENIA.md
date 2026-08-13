# Lenia

Conway's Game of Life with the discreteness taken out — of state, of space, and of time. The same
four-rule skeleton, made continuous, grows things that swim.

**[▶ Open it](lenia.html)** — or download `lenia.html` and double-click it. No install, no server.

## The idea

Life asks each cell a yes/no question about eight integer neighbours. Lenia asks each cell a
real-valued question about a whole neighbourhood, and answers with a small nudge instead of a verdict.

**State.** Every cell holds a number in `[0,1]` instead of dead/alive. The world is a torus.

**Neighbourhood → kernel.** Instead of counting 8 neighbours, take a weighted average over a disc of
radius `R`, using a ring-shaped kernel `K` that sums to 1. Weight depends only on distance:

```
K(r) = β⌊rB⌋ · core((rB) mod 1)     core(u) = exp(4 − 1/(u(1−u)))
```

One ring is a single fuzzy annulus. `β = [1, ⅔, ⅓]` is three concentric rings of decreasing weight.
The ring recipe is what decides which species you get.

**Potential.** Convolve world with kernel: `U = K ∗ A`. Each cell learns how crowded its
neighbourhood is, at the scale the kernel cares about.

**Growth.** "Born on 3, survive on 2–3" becomes a smooth bell centred at `μ` with width `σ`, mapping
potential to a change in `[−1, +1]`:

```
G(u) = 2·exp(−(u−μ)² / 2σ²) − 1
```

Just right → grow. Too sparse or too crowded → shrink. `σ` is the tolerance, and the most sensitive
knob in the system.

**Time.** Don't overwrite the board — increment it:

```
A ← clip(A + (1/T)·G(K ∗ A), 0, 1)
```

At `T = 10` a cell needs ten steps to cross its full range, so structures deform instead of
flickering.

Five parameters — `R`, `T`, `μ`, `σ`, `β` — and that's the whole system.

## Why anything interesting happens

The kernel makes a cell want a specific amount of mass at a specific distance. A ring of the right
thickness feeds its inner edge while starving its outer edge, so it advances; break the symmetry and
it turns. What survives are **solitons**: blobs that repair themselves after damage, collide, split
and orbit. Nobody designed them. They are the only stable solutions to the equation, and rule-space
is mostly desert around them.

## Conway is a special case

Take a 3×3 kernel with the eight neighbours at weight 1 and the centre at weight ½, normalise by 8.5,
use the hard step growth function with `μ = 3/8.5`, `σ = 0.5/8.5`, set `T = 1`, and start from binary
states. Lenia then reproduces the Game of Life exactly — cell for cell, generation for generation.
That's the **Conway limit** preset, and it was checked against an independent Life implementation for
24 generations before shipping.

## Presets

**Creatures** — Orbium · Critters · Amoeba · Worms
**Textures** — Labyrinth · Spores · Plankton · Membrane · Tissue · Cell rings · Coral · Hydrogeminium
**Chaos** — Bubbles · Fissure · Conway limit

Orbium and Hydrogeminium are Bert Chan's published species. The rest were found by search: roughly
1,900 random rules were simulated off-screen, measured, and the survivors inspected one by one; the
ones that held their character across different random seeds got names.

Try **Critters** and **Amoeba** back to back. Identical `μ`, `σ`, `β`, `core`, `growth` — only `R`
and `T` are doubled. One gives a field of small independent creatures; the other gives a single giant
organism with a bright membrane and a churning interior. That is scale-invariance made visible.

## Finding your own

Rule-space is mostly dead or mostly flooded, so the app hunts for you. **Find life** picks random
parameters, simulates each candidate on a 128×128 scratch world, bails early if it dies or floods,
and measures what survived: fill fraction, connected-body count, wall thickness relative to `R`,
spatial occupancy and churn. Those numbers get classified — and you pick the target:

| Target | What it looks for |
|---|---|
| solitons | small, localised, moving |
| worms | thin filaments, thickness well under `R` |
| spores | many small bodies that never merge |
| colonies | solid space-filling growth |
| chaos | high churn, boiling |
| anything alive | neither dead nor flooded |

A match has its `R` and `T` doubled before being handed to the display world, so it arrives at a
visible size. **Mutate** then walks around whatever you found — μ, σ, R, T and β all nudged a little.
That loop, hunt then mutate, is how new species actually get discovered.

## Rendering

The default **spectrum** palette runs dark navy → blue → cyan → green → yellow → red → magenta →
white across `A ∈ [0,1]`, which is roughly how Lenia is rendered in the literature: the interior of a
creature, its membrane and its faint halo are all different hues, so the internal structure is
readable rather than a single glowing mass. Five more palettes are included. The kernel preview uses
the same map, so it always reads at the same scale as the world.

## Controls

| | |
|---|---|
| **Space** | play / pause |
| **←** **→** | step a generation |
| **R** / **Shift+R** | re-seed / randomize the rule |
| **S** **C** | random soup / clear |
| **M** **F** | mutate / find life |
| **P** **?** | toggle panel / how it works |
| drag | paint — hold **shift** or right-drag to erase |

## Performance

The convolution runs through an FFT, so **kernel radius is free** — `R = 40` costs exactly what
`R = 4` costs. The transform exploits the field being real-valued: rows are packed two at a time into
one complex transform, only half the columns are needed by Hermitian symmetry, and the kernel
spectrum is real so the spectral multiply is halved. Everything is `Float32`. That is about 2.2×
faster than the straightforward complex-FFT version.

The whole simulation runs in a Web Worker, so painting and the controls stay responsive while a large
world grinds. Blob workers are blocked on some `file://` setups; if the worker doesn't report in, the
engine transparently falls back to running inline on the main thread.

| Grid | Cells | Speed |
|---|---|---|
| 128×128 | 16k | ~420 steps/s |
| 256×256 | 65k | ~150 steps/s |
| 512×512 | 262k | ~32 steps/s |
| 1024×1024 | 1.05M | ~7 steps/s |

Step-back history is budgeted by memory: 80 states at 128², 6 at 1024².

## Verification

Per house rule 5, the maths was checked before anything was called emergent:

- FFT convolution matches a naive circular convolution to 1.5e-7 (single precision).
- Orbium survives 300 steps with mass stable at 75.1 → 75.9 and travels 15.6 cells.
- The Conway limit is bit-identical to a reference Life implementation over 24 generations.
- Every preset was run from multiple random seeds at 256² and kept only if it neither died nor
  flooded in any of them.

## Things worth trying

- **Orbium**, then drag `σ` from 0.017 to 0.019. It dies. That fragility is the honest story of Lenia.
- Paint into a running **Critters** world — the creatures will eat, deflect or absorb what you draw.
- **Find life** set to *worms*, then **mutate** five times in a row without re-seeding.
- Watch the growth plot while it runs: the bars are a live histogram of `U`, so you can see exactly
  which cells are sitting inside the growth window and which are starving.
- **Labyrinth** on a 512 world at 200 steps/s.
