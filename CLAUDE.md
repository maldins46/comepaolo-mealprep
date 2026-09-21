# CLAUDE.md

Guide for Claude Code (or other agents) working in this repo.

## What this project is

`index.html` is the whole app: a single, self-contained page, no build
step. It generates a weekly meal plan (breakfast/lunch/snack/dinner)
consistent with a personal nutrition plan, and the corresponding shopping
list. No backend, no network calls: all state lives in the browser's
`localStorage`.

There are no other source files: no `src/`, no bundler, no
`package.json`. Changes are made directly to `index.html`.

## How to work on the code

- **One file only.** CSS in `<style>`, JS in `<script>` at the end of
  `<body>`. Keep this structure: no external imports besides the Google
  Fonts already in use.
- **No external libraries**, no CDN other than the Google Fonts already
  linked in `<head>`. The project must remain openable offline via
  `file://`.
- **State**: a single in-memory object `S` (`{settings, days, checked}`),
  saved with `store.set(LS_CUR, S)` on every `render()`. `S.days` is an
  array of 7 elements (one per real day, fixed index 0=Monday...6=Sunday),
  each with the slots `{b,l,s,d}`. The *visual* order of the columns
  (which can start from a day/meal other than Monday morning) is computed
  by `weekOrder()` / `colIndex()` / `slotsOf()`: don't touch the `S.days`
  array to change the order, always go through these functions.
- **Nutrition data**: foods in `FOODS` (values per 100 g/ml, or per piece
  with `u:'pz'`), recipes in `TPL` (templates) plus the `TPL_GROUPS`
  groups, snacks in `SNACKS`. The "target" portions per meal are computed
  in `protItems()`, `carbAmt()`, `vegSplit()`: if you change the plan's
  quantities, also update the text of the "Target" boxes in the editor
  (`targetsBox(...)` inside `openEditor`), which is hand-written and not
  generated from the numbers above.
- **Render**: `render()` is the single entry point that redraws
  everything (settings, board, shopping list) and persists state. After
  any mutation of `S`, call `render()` (the existing handlers in
  `document.addEventListener('click', ...)` usually already do this).
- **Migrations**: `migrate()` handles compatibility with weeks saved
  under previous schema versions (e.g. renaming old fields). If you
  change the shape of `S.settings` or `S.days[i][k]`, add the conversion
  from the old format here, not just the new default.

## Style constraints (important)

- Copy in **Italian**, direct and practical tone, like the rest of the
  UI.
- Weights are always **raw/uncooked** and for **one serving**; state it
  where relevant, don't assume it's obvious.
- Don't introduce a second color/badge language: the colored "bands"
  (`--carne`, `--pesce`, `--uova`, `--legumi`, `--speciale`, `--neutro`)
  always indicate the protein/meal type, consistently across cards,
  editor, and legend.

## Manual testing

There are no automated tests. To verify a change:
1. Open `index.html` in a browser (even just by dragging it into a tab).
2. Check: week generation, opening a meal's editor (mix & match and
   quick options), drag & drop between two compatible slots, Shopping
   list tab, export to Google Keep, saving/opening a saved week, mobile
   view (~390px wide), print.
3. If Playwright is available, a headless script that opens the file and
   checks the `console` for JS errors is the fastest way to catch syntax
   issues after a change.

## What NOT to do

- Don't add a backend or a network call: it's a project requirement, not
  a technical limitation.
- Don't move nutrition data into a separate JSON file unless explicitly
  requested: keeping it inline keeps the project a single file, easy to
  open anywhere.
