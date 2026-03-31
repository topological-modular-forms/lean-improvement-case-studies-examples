# Lean Improvement Case Studies & Examples

A personal knowledge base of Lean 4 code improvement patterns, case studies, and design rationale.

**This project was designed, built, and is curated by Claude Code.**

## What's in here

Each entry is one of:

- **Before/after comparisons** — a piece of Lean code and its improved version, with explanation of what changed and why
- **Design rationale** — discussion of why code is written a certain way, without necessarily showing a diff
- **Mixed** — narrative explanation with optional code examples

Entries are tagged, searchable, and browsable.

## Viewing the site

Open `index.html` in a browser. It loads `db/cases.db` via [sql.js](https://github.com/sql-js/sql.js/) (SQLite compiled to WebAssembly), so no server is needed.

To serve it locally (e.g., for `fetch` to work with `file://` restrictions):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Or host the repo on GitHub Pages — it works as-is.

## Adding entries

Ask Claude Code to insert entries into the database. Entries are managed via the `sqlite3` CLI.

Example:

```bash
sqlite3 db/cases.db "INSERT INTO entries (title, tags, description, before_code, after_code, source_link)
VALUES (
    'Use omega instead of manual arithmetic proofs',
    'tactics,omega,automation',
    'The omega tactic can solve linear arithmetic goals automatically...',
    'theorem foo : 2 + 3 = 5 := by norm_num',
    'theorem foo : 2 + 3 = 5 := by omega',
    'https://github.com/leanprover/lean4/...'
);"
```

## Database schema

Single table `entries`:

| Column | Type | Description |
|--------|------|-------------|
| `id` | INTEGER | Auto-incrementing primary key |
| `title` | TEXT | Entry title (required) |
| `tags` | TEXT | Comma-separated tags |
| `description` | TEXT | Markdown prose |
| `before_code` | TEXT | Original code (optional) |
| `after_code` | TEXT | Improved code (optional) |
| `source_link` | TEXT | URL to source (optional) |
| `source_date` | TEXT | Date of original discussion (YYYY-MM-DD) |
| `rating_interesting` | INTEGER | How interesting (1-10) |
| `rating_important` | INTEGER | How important (1-10) |
| `rating_advanced` | INTEGER | How advanced (1-5) |
| `gerby_tag` | TEXT | Gerby-style tag (e.g., `000P`) |
| `is_reviewed` | INTEGER | Whether Emily has reviewed this entry (0/1) |
| `created_at` | TEXT | Timestamp, auto-set on insert |

### Gerby tag scheme

Tags are 4-character codes using the alphabet `0123456789ABCDEFGHIJKLMNPQRSTUVWXYZ` (base-35, no letter O). They are assigned sequentially starting from `0000`, following the same scheme as the [Stacks project](https://stacks.math.columbia.edu/). Tags are permanent — once assigned, they never change.

## Rating guidelines

Each entry has three ratings. These are designed to help readers find entries appropriate to their level and interests.

### Interesting (1–10): How surprising or insightful is this?

| Score | Meaning | Examples |
|-------|---------|---------|
| 1–2 | Routine, mechanical change. No surprise. | Whitespace fixes, trivial renames |
| 3–4 | Useful to know, but predictable once stated. | Simple post-port cleanup, notation migration |
| 5–6 | Notable insight. Moderately surprising or a useful pattern. | Making a definition irreducible for speed, adding explicit projections |
| 7–8 | Very surprising or elegant. Teaches something non-obvious about Lean. | Extends clause ordering affects performance; removing features improves a tactic; accidental global instances cause slowdowns |
| 9–10 | Genuinely mind-blowing or counter-intuitive. Changes how you think about Lean. | Renaming a universe variable gives 200x speedup; the mixin philosophy that drove years of refactoring |

### Important (1–10): How much does knowing this matter for practical Lean development?

| Score | Meaning | Examples |
|-------|---------|---------|
| 1–2 | Niche, affects very few users or files. | A specific workaround for one file |
| 3–4 | Relevant to a specific mathematical area. | Number theory-specific instance speedup |
| 5–6 | Broadly relevant to many mathlib contributors. | API design principle, tactic usage pattern |
| 7–8 | Essential knowledge for regular mathlib contributors. | Instance priority patterns, simp robustness, naming conventions |
| 9–10 | Fundamental principle that shapes all Lean 4 development. | The mixin typeclass philosophy, the one-field structure pattern for concrete categories |

### Advanced (1–5): How much Lean expertise is needed to understand and apply this?

| Score | Meaning | Prerequisites |
|-------|---------|---------------|
| 1 | Beginner | Basic Lean syntax, simple tactic proofs |
| 2 | Intermediate | Typeclasses, `simp`, `rw`, common mathlib patterns, basic API design |
| 3 | Advanced | Elaboration strategy, instance synthesis, transparency levels, API design principles |
| 4 | Expert | Kernel definitional equality, discrimination trees, universe unification, metaprogramming |
| 5 | Lean internals | Lean 4 compiler/kernel implementation, elaborator internals, custom elaborators |

## TODO

- [ ] Mine discussions from large formalization projects (FLT, Carleson, sphere eversion, Brownian motion, PFR, etc.) for improvement case studies specific to large-scale formalization challenges
- [ ] Process remaining Tier 3–5 PRs (~5,000 more)
- [ ] Cross-reference full Zulip archive (46k topics) with PR data to find discussions not linked to any PR
- [ ] Add entries from lean4 repo issues/PRs (not just mathlib4)
- [ ] Fetch and process full comment threads for all PRs with discussion (~22 hour job)

## Features

- Full-text search across titles, descriptions, and tags
- Tag filtering via clickable pills
- Light/dark theme toggle (preference saved in localStorage)
- Lean syntax highlighting via highlight.js
- Markdown rendering in descriptions
- Responsive layout
- Zero server dependencies — runs entirely in the browser

## Tech stack

- **sql.js** — SQLite in WebAssembly
- **marked.js** — Markdown rendering
- **highlight.js** — Syntax highlighting (Lean language)
- **Alegreya Sans** — Body font via Google Fonts
