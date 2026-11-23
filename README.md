# Faster makefun-site

[![bounty](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/Bruskots/makefun-site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/Bruskots/makefun-site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, written for make.fun's
[Build a static site faster](https://make.fun/bounty/site-generator-speed-01-run-20261122)
bounty ($100 in USDC).

`run(input)` in `solve.js` returns exactly the answer makefun-site 1.0.0
returns for a site — every page, index, tag page, `build-report.txt` and
static file, byte for byte — but faster: on the practice set it runs in about
a quarter of the house baseline's CPU time (roughly 4x faster).

## How it is faster

- One line-split per source, shared by the front-matter and Markdown passes.
- A Markdown renderer that copies plain runs in bulk instead of escaping
  character by character, and renders each block straight into one joined
  array.
- Cached slugs and long dates (pure functions, cleared per case).
- Templates parsed once into small opcode arrays and rendered into a joined
  array; includes whose output can only depend on the case-wide values
  (`site`, `tags`, `recent`) are rendered once per case and reused.
- The word count behind `reading_minutes` is counted once per source with a
  whitespace lookup table.

## Layout

- `solve.js` — the entrypoint: a dependency-free ES module exporting `run(input)`.
- `main.ts` — the driver the harness runs: reads `/input/cases.json`, answers
  each case with `run`, writes `/output/answers.json`.

## License

MIT. See [LICENSE](LICENSE).
