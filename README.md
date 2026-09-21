# Comepaolo Meal Prep

Meal and grocery planning tool, tuned to my nutrition goals.

A single HTML page, no build and no dependencies to install: generates a
weekly plan for breakfast/lunch/snack/dinner, keeps it balanced for kcal
and macros, and produces the corresponding shopping list.

## Usage

Just open `index.html` in a browser. No server needed: it also works by
opening the file directly from the filesystem (`file://...`).

To publish it online, see [Deploy](#deploy) below.

## Features

- **Generate week**: builds a full plan following the nutrition plan's
  rules (protein/carbs/veggies per meal, training days, "leftover" lunches
  linked to the previous day's dinner, weekend grain batches, etc.). Manual
  edits stay in place when you regenerate.
- **Editor for each meal**: free mix & match (protein, carb, veggie,
  cooking method, condiment) or quick pick among ready-made recipes,
  leftovers, and free meals. Each option shows raw weights and full macros.
- **Drag & drop**: drag a meal onto another to swap them (mouse and
  touch).
- **Movable free meals**: "free meal at the office" and "free meal" can be
  placed on any lunch or dinner slot of the week; the day's calorie target
  adjusts accordingly.
- **Grain batch and protein stock**: calculates what to cook ahead,
  automatically splitting between fridge (a few days) and freezer (beyond
  2-3 days).
- **Shopping list**: grouped by department, with checkboxes, quantities
  aggregated across the whole week, and notes for portioning/freezing.
- **Export for Google Keep**: copies the plan or list in a format that,
  pasted into a new Google Keep list, splits itself into items with
  native checkboxes.
- **Saved weeks**: local save in the browser (localStorage), to resume or
  reuse a week later.
- **Print**: dedicated layout for printing the plan and list.

## Stack

Vanilla HTML/CSS/JavaScript in a single file (`index.html`). No external
dependencies, no bundler, no network calls: all data stays in the browser
(`localStorage`), nothing is sent elsewhere.

## Customization

Nutrition data (foods, portions, recipes, per-meal targets) is all in the
first ~350 lines of `index.html`, in the `FOODS`, `TPL` (recipe templates),
`SNACKS`, `PROTS`, etc. objects. To adapt the tool to a different plan,
just edit these objects: the UI adjusts on its own.

## Deploy

Any static hosting works, for example:

**GitHub Pages**
1. Settings → Pages → Deploy from a branch → branch `main`, folder `/root`.
2. The page will be at `https://<user>.github.io/<repo>/`.

**Netlify Drop**
Drag the folder onto https://app.netlify.com/drop.

## Privacy

All state (current week, saved weeks, list checkmarks) lives in the
browser's `localStorage` of whoever opens the page. There's no backend:
if you share the link or file, whoever opens it starts from scratch,
they don't see your plan.

## License

Personal use. Add a license (e.g. MIT) if you want to make the repo
public and reusable by others.
