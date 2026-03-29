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
| `created_at` | TEXT | Timestamp, auto-set on insert |

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
