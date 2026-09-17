# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Panel Digest — a marketing/landing site for a comic-based current-affairs product aimed at schools. There is no app code here, only the site.

## Architecture

The entire site is a single self-contained HTML file: `index.html` (~1450 lines: inline `<style>`, markup, inline `<script>`, no external JS files, no build step, no dependencies besides two Google Fonts links).

- `1.png` / `2.png` are the actual comic art for "Issue 01" (the six-panel explainer and its companion revision quiz), referenced directly by the HTML.
- Deployed to Vercel as a static site (`.vercel/project.json`); no `vercel.json`, no framework — Vercel serves the HTML/PNG files as-is.

### Page structure (all in `index.html`)

Three tab-switched views inside one document, toggled by `showView(name)` in the inline script (no router, no reload):
- `#view-catalog` — the issue catalog and the full "Issue 01" spread (comic + quiz + rewards).
- `#view-analytics` — static SVG charts (hand-authored `<svg>`, not a charting library) citing external research.
- `#view-pricing` — pricing card + in-production consumer app mockups (fake phone screenshots built from CSS/HTML, not real images).

### Comic fullscreen viewer

`PAGE_DATA` in the inline `<script>` hand-maps pixel rectangles within `1.png` to "pages" (rows of panels) for the mobile fullscreen reader (`initComicFullscreen`). Each rect is `{ x, y, w, h }` in source-image pixels, grouped by page, and rendered as background-image crops (`background-size`/`background-position` math) stacked vertically. If the source art (`1.png`) changes or new issues are added, `PAGE_DATA` coordinates must be re-measured against the new image and a new entry keyed by filename added.

### Theming

CSS custom properties on `:root`, with dark-mode overrides both via `@media (prefers-color-scheme: dark)` and an explicit `:root[data-theme="dark"]` override (no toggle UI currently wired up, but the CSS supports one). Per-issue "palette" colors (`--comic-*`, `--quiz-*`) are sampled by eye from each issue's own art rather than reused across issues — new issues should get their own sampled palette, not the existing one.

## Working in this repo

- No build/lint/test tooling exists. Verify changes by opening `index.html` directly in a browser.
- Edits are plain HTML/CSS/JS in one file — keep changes inline rather than introducing a build step or framework for a static page like this.
