# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This repository is a single static HTML file (`index.html`) implementing an abstract photography portfolio landing page. There is no build system, package manager, test suite, or linter — the entire site is one self-contained file with inline `<style>` and `<script>` blocks.

## Commands

There are no build, lint, or test commands — there is nothing to compile or check. To preview the page, open `index.html` directly in a browser, or serve the directory with any static file server, e.g.:

```
python3 -m http.server
```

## Architecture

Everything lives in `index.html`, structured in three layers:

- **Background layer** (`.background`, `.blob`): a fixed, negatively-z-indexed radial-gradient backdrop plus three absolutely-positioned blurred `.blob` divs animated via the `float` keyframe (defined by CSS `:nth-child` position/color, not markup attributes).
- **Content layer**: a full-viewport `.hero` section followed by a `.gallery-section` containing `.photo` cards laid out with CSS multi-column (`columns: 3 300px`), so card order in markup determines column flow, not a grid. Hover states (image zoom, gradient overlay, caption reveal, colored box-shadow) are pure CSS, split across two non-adjacent `.photo` rule blocks (lines ~117 and ~175) — the second block's `transform`/`transition` declarations override the first's, so both must be read together to know the final hover behavior.
- **Parallax script**: a single `requestAnimationFrame` loop (`smoothScroll`) lerps `window.pageYOffset` toward a `currentScroll` value and applies `translate3d` to every `.parallax`-classed element, scaled by that element's `data-speed` attribute (falling back to `0.1`). None of the current elements set `data-speed`, so all parallax elements currently move at the same default speed — adding differentiated scroll speed to an element means adding `data-speed="<value>"` to its markup, not touching the script.

Gallery images are hotlinked from Unsplash (`images.unsplash.com/...?w=1200`); there are no local image assets.
