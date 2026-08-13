# Simple HTML Games

A collection of cellular automata you can actually play with. Every entry is a **single
self-contained HTML file** — no build step, no npm install, no server. Download it, double-click it,
and it runs.

The premise: cellular automata are usually presented as demos you watch. These are built as
instruments you operate — load your own images, paint cells, stamp patterns, change the rules while
they run, and see what falls out.

## In the collection

| | Automaton | What it is |
|---|---|---|
| 🧬 | **[RGB Life](rgb-life.html)** | Conway's Game of Life played on the pixels of your photos, with 3 or 6 competing species and nine cross-species interaction models |
| ◽ | **[Rule X](rule-x.html)** | All 256 elementary (Wolfram) rules, edited by clicking the eight answer boxes; plus four-cell neighbourhoods and 65,536 rules |
| 🌀 | **[Lenia](lenia.html)** | Conway's Life with state, space and time made continuous; grows lifelike gliders, plus a search that hunts rule-space for new species |

*More on the way — see [Ideas](#ideas) below.*

## Running them

**Easiest:** download any `.html` file here and open it in a browser. That's it.

**Live:** if GitHub Pages is enabled on this repo, everything is browsable at
`https://ayushchinmay.github.io/Simple-HTML-Games/`.

Everything runs entirely in your browser. No data leaves your machine — images you load are decoded
locally and never uploaded anywhere.

## House rules for new entries

So the collection stays coherent as it grows:

1. **One HTML file per automaton** at the repo root, plus a `NAME.md` write-up beside it.
2. **Single file, no build.** Vanilla JS, no bundler, no framework. Google Fonts over CDN is the only
   external request, and everything must still work with it blocked.
3. **No browser storage.** Some browsers block `localStorage` on `file://` pages, which breaks the
   double-click-to-run promise. Persist by exporting a file instead.
4. **Ship the controls.** Play/pause, speed, step, reset, and a way to draw. If a rule has a
   parameter, expose it — the point is to fiddle.
5. **Verify the rules.** If an automaton claims a pattern is a period-3 oscillator, run it and check.
   Bugs in a simulator are invisible; wrong output just looks like emergence.
6. Add a row to the table above and a card to `index.html`.

## Ideas

Candidates for future entries, roughly easiest first:

- **Langton's ant** — one ant, two rules, and a highway that appears after 10,000 steps of chaos
- **Brian's Brain** — three states (firing, refractory, dead); produces relentless drifting gliders
- **Wireworld** — electrons in circuits; build real logic gates, adders, even a prime sieve
- **Cyclic cellular automaton** — n states in a dominance loop; famous for spontaneous spirals
- **Abelian sandpile** — drop grains, watch avalanches settle into fractal patterns
- **Turmites** — Langton's ant with a bigger state table, in two dimensions
- **Reaction–diffusion (Gray–Scott)** — continuous rather than discrete; coral, fingerprints, spots
- **Falling sand** — sand, water, fire, oil, with interactions; the most game-like of the lot

## License

MIT — see [LICENSE](LICENSE).
