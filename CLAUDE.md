# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static site for m1c2.xyz, the product studio of Zak Hap. The site is drawn as the inside of a wireframe
tesseract: each page of content is a 600 × 600 px square of HTML pinned to a wall, and scrolling turns the
tesseract through 4D to bring the next one in. **HTML and CSS only, no JavaScript** (a deliberate constraint;
ask before adding a script).

**Read `HANDOFF.md` before changing anything.** It covers how the engine works, the design decisions already
made, known problems and the working method (check every pane for overflow at desktop and phone size).

## Files

- `index.html` home (11 panes); `tenets.html` and three essays (`latent-space-as-medium.html`,
  `signatures.html`, `tyranny-of-pre-digestion.html`), paged by hand at ~140 words per pane
- `css/type.css` all typography, layout and colour. This is the file to work in
- `css/engine.css` the tesseract maths. Touch only the knobs in its first `:root` block and the camera media queries
- `fonts/` STIX Two Text; `img/` site images (including `og.png`, the share card)
- `images/` assets from the previous site, no longer referenced; kept so old links don't break
- `feed.xml`, `sitemap.xml`, `robots.txt`, `llms.txt` hand-written; update them when adding an essay

No build step, no dependencies. Open `index.html` in Chrome, or serve the folder.
