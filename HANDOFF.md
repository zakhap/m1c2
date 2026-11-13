# Tesseract site: handoff notes

Written 11 Nov 2026 at the end of a long design session in the Claude app, then updated the same
day in Claude Code when the redesign was promoted from `tesseract/` to the repo root, replacing the
previous collage-style site (still in git history; its last version is commit 66a69ef).

Open `index.html` in Chrome to see it. No build step, no server needed.

## What this is

The site rendered as the inside of a tesseract (4D hypercube) drawn in straight lines.
Each "page" of content is a square of real HTML pinned to a wall of the room. Scrolling one
viewport height turns the tesseract a quarter-turn through a 4D plane, carrying the next
page in on its own wall.

**Hard constraint, by Zak's choice: HTML and CSS only. There is no JavaScript anywhere.**
The one exception is the `<script type="application/ld+json">` block in each page's `<head>`: it is
structured data for search engines, not code, and the browser never runs it. Keep it.
The 4D rotation, both perspective projections, line clipping and the text warp are all done
in `calc()` driven by one scroll-linked number. Do not add a script to fix something without
asking; the "no JS" property is the point of the piece.

## Files

| Path | What it is | Edit? |
|---|---|---|
| `index.html` | Home: 7 panes (name + social icons, ~daylight, ~moonlight, ~writings, ~app musings, ~speaking, ~elsewhere) | Yes, by hand |
| `tenets.html`, `latent-space-as-medium.html`, `signatures.html`, `tyranny-of-pre-digestion.html` | Essays as paged panes. Text verbatim from the previous site | Yes, by hand |
| `css/type.css` | All typography, layout, colour scheme, heavily commented | **Yes. This is the file to work in** |
| `css/engine.css` | The tesseract. Generated originally, then hand-patched | Only the knobs in the first `:root` block and the camera media queries |
| `fonts/` | STIX Two Text (variable, Latin subset), SIL OFL | No |
| `img/sho.webp` | Web-sized copy of `images/sho1.png`, shown on hover of "shō" | No |
| `img/og.png` | 1200 × 630 share card: a screenshot of the first pane. Retake it if the first pane changes | Regenerate |
| `images/` | The previous site's assets. Nothing references them now; kept so outside links don't break | Can go eventually |
| `sitemap.xml`, `robots.txt`, `llms.txt`, `feed.xml` | Hand-written discovery files | Update when adding an essay |

## How the engine works (enough to not break it)

- `.scene` (the `<main>`) carries `--p`, the scroll position measured in pages (0 = first pane).
  It is animated by `@keyframes travel` on a scroll timeline, from 0 to `--last`.
- **Each HTML file must set `style="--last: N"` on `<main>`, where N = number of panes minus one.**
  Forget this and the turns drift out of step with the panes.
- Each pane is `<section class="stop" id="..."><div class="wall">…</div></section>`. The section is
  in normal flow and 100vh tall, which is what gives the document real scroll positions and working
  `#anchors`. The `.wall` inside is `position: fixed` and gets a `matrix3d()` that maps its
  600 × 600 px box onto the projected corners of a tesseract face.
- **Every wall is a 600 × 600 px box.** All sizes in `type.css` are px of that box. Content that
  is taller than 600 is clipped. There is no automatic flow between panes; long text is split by hand.
  About 140 words of body prose fit on one pane.
- The six turns (z→w, x→w, x→z, y→w, w→z, y→z) repeat cyclically, so a page can have any number
  of panes. `engine.css` has `--n` (page index) rules for up to 48 panes; add more if needed.
- The 32 edges are 32 empty `<i>` elements inside `.lines`. Their endpoints are computed in CSS;
  the per-edge constants are at the bottom of `engine.css`.
- Knobs at the top of `engine.css`: `--cam` (eye position; 2.2 = just outside the tesseract,
  0.55 = inside the inner room), `--focal` (lens), `--ease` (how much each turn eases),
  `--in-at/--in-len/--out-at/--out-len` (when pages fade during a turn).
- Desktop is `--cam: 1.6; --focal: 84` (was 2.2 / 100). On an 823px-high laptop the wall is ~407px tall and
  the outer cube's frame sits clear of the screen edges. 90 (wall ~436px) put the frame right on the edges, and
  Zak asked for a little more room; 100 (wall ~484px) cropped it.
  Raise `--focal` for bigger text, lower it to show more of the tesseract.
- Portrait screens override `--cam`/`--focal` to sit inside the room so the text wall fills ~85% of the width.
- Landscape phones (`orientation: landscape` and `max-height: 500px`) also sit inside the room, with a lens
  that fills the height, and move the scroll cue beside the wall. `type.css` gives them the portrait type sizes.
- Fallback: with `prefers-reduced-motion`, or in browsers without `animation-timeline: scroll()`
  (Firefox by default, Safari before 26), the same HTML shows as a plain stacked document.
- Scroll snapping (`scroll-snap-type: y mandatory` on `html`, `scroll-snap-align: start` on `.stop`) makes every
  page a snap point: the space bar and Page Down land exactly on the next page, and a scroll never rests mid-turn.
  The stacked fallback turns snapping off. Remove those two lines for free scrolling.
- Also in `engine.css`: the browser scrollbar is hidden and a drawn rail (`.rail`, one link per pane,
  must list the page's pane ids) replaces it: click to jump, not draggable, the scroll cue arrow (`.cue`) and the `^top` link.

## Design decisions already made (don't relitigate without Zak)

- **Typeface:** STIX Two Text. Chosen after trying twelve; it is "the right font for the moment".
- **Colour:** white lines and text on cobalt `#4a5fa5`. Set in one marked block at the bottom of `type.css`.
- **All text lower case**, via one CSS rule at the bottom of `type.css`, **except running prose**
  (`.prose`, `.say`, block epigraphs), which keeps its capitals. Decided by Zak 11 Nov 2026. The HTML keeps real capitals.
- **Hierarchy is deliberately calm.** Zak found the earlier big-type version "dramatic". Largest type
  is a little over twice the smallest. Scale: 19 / 22 / then 24–44 for names.
- **Layout is a 6-column grid** with rows that fill each pane top to bottom, ruled edge to edge.
  Pane titles are small, lower-case, with a leading tilde (`~daylight`).
  The title band is 58px with a rule under it, edge to edge like the row rules (tightened from 78px with no rule, 11 Nov 2026).
- **Links:** no underline at rest, a small drawn arrow after the text, wavy underline on hover.
- **No page numbers, no colophon line, no "tesseract is css" label.** All tried and removed.
- **Essays:** paged, one pane per ~140 words. The scrolling, crawl and zoom variants were deleted
  (recoverable from the Claude app session if ever wanted).
- **Links inside running text** (descriptions, captions) have a hairline underline instead of the arrow.
- **"recent" box** on the first pane, between the name and the contents: a dashed box with up to two items
  (link + one small line), each with a blinking "new!" hanging just left of its name. On a phone there are only ~24px to spare,
  so a third item or a longer line will clip the contents list. The bold "Measure once, cut twice." was removed from the intro.
- **No asides.** The old site's asides (Minerva drawing, Scriabin story, machine vision, soup) were ported as
  panes, then removed on 11 Nov 2026 to trim the site before new projects go in. They are in git history.
- **Social icons** (X, GitHub, Are.na, RSS) sit at the far right of the name on the first pane: inline SVG,
  white via `currentColor`, no arrow. The ~elsewhere pane keeps the same links in full. The Are.na mark is
  their official logo path, taken from the inline SVG on are.na's home page.
- **Scroll cue arrow stays visible between panes**; it only fades on the last pane.
- Years on the ~daylight pane were supplied by Zak: Liminal Media 2018, Odefi 2019,
  Common 2019–2024, Hit One 2025–2026.

## Things to know about the content

- **Home copy** now follows the previous site's `index.html` (commit 66a69ef), condensed to fit the panes:
  the daylight figures ($600M+, $1B+, ~$50M), the Study Hall and Qubit venues, the Charlie Freiberg link.
  "BAM Fisher - Fishman Space" is shortened to "BAM Fisher". The Scriabin story's "I was one told" is
  corrected to "I was once told" (that pane has since been removed).
- `signatures.html`: the third paragraph is too long for one pane, so it is split across two
  panes at "Perhaps." That is a layout split, not an edit to the text.
- Possible typos in the source essay "Latent Space as Medium", left untouched: "a lot time",
  "the LLMs probabilistic prediction", "envelopes us".
- `about.html` was deleted from the old site in Aug 2026 and its content folded into the home page;
  that content is now on the home panes. The shō hover is done without JS on ~moonlight (the photo shows at 460px tall over the label column). ~moonlight no longer has the star tile background.

## Known problems

1. ~~Landscape phones~~ fixed: own camera rule. Text is ~13px; a square wall on a 390px-tall screen can't do much better.
2. ~~Lower case in prose~~ fixed: prose keeps its capitals.
3. ~~Small text on laptops~~ improved: ~15px with `--cam: 1.6`. Go to 1.4 for bigger, but the cue then overlaps the wall.
4. ~~Styled scrollbar missing in the Claude app's viewer~~ moot: the browser scrollbar is now hidden everywhere
   and the drawn `.rail` is the only scrollbar.
5. **Mid-turn on portrait phones** the two visible walls are cropped at the screen edges.
6. On the two turns where a wall passes through the viewer (panes 1→2 and 5→6), outgoing and
   incoming text overlap briefly.
7. **Safari bug, fixed 11 Nov 2026:** Safari 26 (macOS and iOS) miscomputes `tan(atan2(1vmin, 1px))`, which the engine
   used to get px-per-vmin (`--vpx`). The walls drew at ~1/16 size while the lines were fine. `engine.css` now uses
   `calc(1vmin / 1px)` wherever `@supports` says the browser can divide lengths (Safari 26, Chrome), and keeps the
   trig trick only as a fallback. Still not tested: Firefox (gets the stacked fallback), a screen reader.
   Earlier note: not tested: real Safari, real iOS, Firefox, a screen reader. Testing so far was headless
   Chrome screenshots at 1512 × 823 and 390 × 844.
8. `engine.css` is ~500 lines of generated `calc()`. The generator was a throwaway Python script
   and was not kept. If you need to change the maths, edit the CSS directly; the structure is
   regular (one block per line endpoint A/B, one block per wall corner C0–C3).

## Suggested next steps

1. Deploy and check the live site: social unfurl (og.png), RSS, old URLs.
2. Test on a real iPhone (portrait and landscape) and in Safari 26. Firefox gets the stacked fallback.
3. Decide whether `images/` can be deleted (nothing on the site uses it).
4. Optional: the custom arrow cursor Zak liked was only built in the explorer artifact, not here.

## Working method that held up

- Change `type.css`, reload, look. Check every pane for overflow at desktop and phone size before
  calling it done: a pane silently clips anything past 600px.
- When adding a pane: add the `<section class="stop" id="…">`, bump `--last`, and add its id to `.rail`.
- Keep the HTML clean. Per-pane engine constants live in `engine.css`, keyed by position, not in markup.
