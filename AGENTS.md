# AGENTS.md

## Project

Typednotes homepage, published with GitHub Pages at https://www.typednotes.com.

- `index.html`: the whole site, self-contained (inline CSS/JS, no dependencies, no build step).
  Canvas-rendered ASCII-art animation: 3D nodes (Lean 4 / Calculus of Constructions terms)
  moving uniformly and linked to their neighbours, with a "Typednotes" title overlay.
- `privacy.html`, `terms.html`: privacy policy (required for Google OAuth verification) and
  terms of service. Each is a separate self-contained static file: light blue console with a
  blue ASCII-art 3D graph (rotating on `privacy.html`, forward flight with vertices appearing in
  the distance on `terms.html`) behind a white, scrollable content window. Served as
  `/privacy` and `/terms` by GitHub Pages.
- All three pages share a bottom nav linking home / privacy / terms.
- `.github/workflows/pages.yml`: deploys the three HTML files to GitHub Pages on push to `main`
  (add any new page to its "Assemble site" step).
- `README.md`: Pages and DNS setup.

Local preview: `python3 -m http.server 8000` then open http://localhost:8000.

## Git workflow

- Agents may create commits when asked, but must **never push**. The user prefers to push
  themselves.
