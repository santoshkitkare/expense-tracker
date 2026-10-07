# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a small Flask expense tracker (INR, `₹`). It is a step-by-step teaching project: several routes and the database layer are deliberate stubs that are filled in later ("Step N" in the placeholder strings).

## Commands

Windows; a virtualenv lives in `venv/` (git-ignored).

```
venv\Scripts\activate
pip install -r requirements.txt      # flask, werkzeug, pytest, pytest-flask
python app.py                        # dev server, debug on, http://localhost:5001
pytest                               # run tests (none exist yet)
pytest path/to/test_file.py::test_name   # single test
```

No linter or build step is configured. The app uses port 5001, not Flask's default 5000.

## Architecture

- `app.py` — single Flask app; all routes live here. Implemented: `/`, `/register`, `/login`, `/terms`, `/privacy` (each just renders a template; the register/login pages have no form handling yet). Placeholder routes returning plain strings: `/logout`, `/profile`, `/expenses/add`, `/expenses/<id>/edit`, `/expenses/<id>/delete`.
- `database/db.py` — empty stub describing the intended API: `get_db()` (SQLite connection with `row_factory` and foreign keys on), `init_db()` (`CREATE TABLE IF NOT EXISTS`), `seed_db()`. `expense_tracker.db` is git-ignored. Nothing imports this module yet.
- `templates/base.html` — shared layout (navbar, footer with Terms/Privacy links, `head` / `content` / `scripts` blocks). Every page extends it and links with `url_for(...)`, so a new route's endpoint name is what templates reference.
- `static/css/style.css` — the only stylesheet, linked from `base.html`. All theming is CSS variables in `:root` (`--ink`, `--paper`, `--accent`, `--font-display`, ...); reuse them rather than hard-coding colours. Responsive rules are grouped at the bottom.
- `static/js/main.js` — essentially empty. Page-specific JS is written inline in the page's `{% block scripts %}` (see the video modal in `landing.html`). No JS frameworks or libraries are used; keep it vanilla.

## Conventions

- Page-specific CSS that doesn't belong in `style.css` goes inline in the template's `{% block head %}` (`terms.html` and `privacy.html` share an identical `.legal` card style block).
- There is no `static/css/landing.css`; landing/hero styles are in `style.css`.
