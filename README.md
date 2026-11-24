[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/arash2105/site-generator-fast/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/arash2105/site-generator-fast/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

# site-generator-fast

A faster drop-in for make.fun's own site builder, makefun-site 1.0.0.

`solve.js` exports `run(input)`, which returns exactly what makefun-site 1.0.0
returns for one case's input: the site's output tree as a JSON object of every
path it writes and that file's text, byte for byte — every page's `index.html`,
the index and tag pages, `build-report.txt` and every static file. It is a
single self-contained file: no imports, no network, no clock, no randomness,
and nothing carried from one case to the next.

It is faster because it removes work the reference repeats: slugs are
memoised within one run (their NFKD normalisation is slow), words are counted
by scanning instead of a match array, each page's card is built once instead
of twice, Markdown lines are classified by their first character instead of
running every pattern on every line, and files are only normalised for line
ends when they actually hold a carriage return. Every edge case of the
reference is kept: front-matter refusal reasons and their order, slug
uniqueness with the reserved `posts` / `tags` / `page` directories, the
Markdown subset (not CommonMark), the template dialect with standalone-tag
line removal and include depth 8, broken-link reporting, and the report's
sorted lines and counts.

`main.ts` is the program the harness runs: it reads every case from
`/input/cases.json`, answers each one alone with `run()`, and writes
`/output/answers.json`.

## Run locally

```sh
node main.ts
```

With custom directories:

```sh
MAKEFUN_INPUT_DIR=./input MAKEFUN_OUTPUT_DIR=./output node main.ts
```
