# AGENTS.md

## Project

Typednotes homepage, published with GitHub Pages at https://www.typednotes.com.

- `index.html`: the whole site, self-contained (inline CSS/JS, no dependencies, no build step).
  Canvas-rendered ASCII-art animation: 3D nodes (Lean 4 / Calculus of Constructions terms)
  moving uniformly and linked to their neighbours, with a "Typednotes" title overlay.
- `.github/workflows/pages.yml`: deploys `index.html` to GitHub Pages on push to `main`.
- `README.md`: Pages and DNS setup.

Local preview: `python3 -m http.server 8000` then open http://localhost:8000.

## Git workflow

- Agents may create commits when asked, but must **never push**. The user prefers to push
  themselves.
