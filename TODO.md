# TODO

Deferred work on m1c2.xyz. Nothing here is blocking; grouped by what it buys you.

## Discovery — social / link unfurls

- [ ] **`og:image`** — none exists anywhere on the site. Every link posted to X, Slack,
      iMessage, or Discord unfurls as a bare gray box. One reusable 1200×630 card is
      enough: `#2a1f1a` ground, `measure once, cut twice` in the mono face, nothing else.
      Reference it site-wide with `og:image`, `og:image:width`, `og:image:height`, and
      `twitter:image`. Bump `twitter:card` to `summary_large_image` once it exists.
- [ ] **OG / Twitter tags on subpages** — only `index.html` has any. `about.html`,
      `tenets.html`, and all three essays have zero, so they unfurl with whatever the
      crawler guesses. Each needs `og:type` (`article` for essays), `og:title`,
      `og:description`, `og:url`.
- [ ] **`<link rel="canonical">`** — absent site-wide. Cheap insurance against
      `m1c2.xyz` / `www.m1c2.xyz` / trailing-slash variants splitting ranking signal.

## Discovery — search engines

- [ ] **`sitemap.xml`** — 6 pages, hand-written, no build step needed.
- [ ] **`robots.txt`** — currently absent, so crawler behavior is entirely default.
      Should at minimum point at the sitemap.
- [ ] **JSON-LD** — `Person` on `about.html`, `BlogPosting` on each essay. This is what
      populates knowledge panels and rich results.
- [ ] **Visible dates on essays** — none of the essays display a publication date in the
      page body. Both readers and crawlers currently have no way to tell how old a piece
      is. Pairs with `datePublished` in the JSON-LD.

## Discovery — agents / LLM retrieval

- [ ] **`/llms.txt`** — the emerging convention for telling agents what a site contains
      and where the good stuff is. Small site, so a hand-written one covering the six
      pages is genuinely complete rather than a summary.
- [ ] **Semantic structure in essays** — `tenets.html` renders its tenets as
      `<div class="tenet">` with an `<h2>` inside. Real sectioning elements (`<article>`,
      `<section>`) extract far more reliably than divs when a model is chunking the page.
- [ ] **Descriptive `<title>` on essays** — check each is `Title — m1c2` rather than
      bare. Titles are weighted heavily in both search snippets and retrieval.

## Feed

- [ ] **`feed.xml` is missing `Studio Practice Tenets`** — it's listed under Writing on
      the index but has never appeared in the feed.
- [ ] **`pubDate` values look backfilled** — the three remaining items cluster on
      24 and 26 May 2025. If the real dates are known, use them; if not, this is fine,
      but it's visible to anyone reading the raw feed.
- [ ] **No `<atom:link rel="alternate">` per item, no `<author>`, no `<category>`.**
      Optional, but readers use them.

## Content — pending decision, not started

- [ ] **The Calvino subtraction pass.** Proposed and reviewed but deliberately not
      applied. Covers: `tenets.html` cut from nine tenets to six with the startup
      vocabulary removed; `index.html` `App Musings` → `Small Apps` and a rewritten
      meta description; `about.html` de-duplicated against the index, `Miscellany` →
      `Otherwise`. Awaiting the word before any of this copy is touched.

## Housekeeping

- [ ] **CSS is duplicated inline across all six pages.** Intentional per `CLAUDE.md`
      (self-contained pages, no build step) — noted only because the palette migration to
      `#2a1f1a` had to be made in six places, and the next one will too.
- [ ] **Two footer idioms coexist.** `tenets.html` uses `#5c4a3d` / `#a37762`; the three
      essays use `#444` / `#888` left over from the pre-migration palette. Worth
      unifying on the warm tones.
