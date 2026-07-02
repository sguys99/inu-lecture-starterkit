---
version: 2.0
name: starter-kit-design
description: A deep-space, dark-first knowledge-graph reader for a personal AI wiki. The whole system is monochrome ink except one luminous-aqua signal color (#5eead4) that carries every bit of emphasis — links, hover borders, the active filter chip, the focus ring, the reading progress bar, and the constellation graph itself. Three fonts split by role: Space Grotesk for Latin display (wordmark, hero stats, eyebrows), Pretendard Variable for Korean + body, JetBrains Mono for meta/tags/code. It is hand-written CSS (no Tailwind, no shadcn) organized with @layer tokens/base/components/utilities; tokens are CSS custom properties. The signature is an animated constellation canvas hero and a card-grid feed whose hover highlights each card's graph neighbors. Rendered as a static site (build.mjs → GitHub Pages under /ai-wiki), themed dark by default with an opt-in light override via `data-theme` on `<html>`.

colors:
  # Dark theme — the default (:root). "deep space ink"
  bg: "#0b0e14"
  surface: "#141923"
  surface-2: "#1b2230"
  text: "#e6e9ef"
  muted: "#8a93a3"
  faint: "#5a6373"
  hairline: "#232a38"
  signal: "#5eead4"                 # luminous aqua — the single accent
  signal-dim: "rgba(94,234,212,0.25)"  # edge / glow / selection / underline
  # Light theme — opt-in override (:root[data-theme='light'])
  bg-light: "#f7f8fa"
  surface-light: "#ffffff"
  surface-2-light: "#f1f3f7"
  text-light: "#161a22"
  muted-light: "#5c6473"
  faint-light: "#9aa2b0"
  hairline-light: "#e3e7ee"
  signal-light: "#0fb89b"
  signal-dim-light: "rgba(15,184,155,0.18)"

typography:
  fontRoles:
    display: "'Space Grotesk', ui-sans-serif, system-ui, sans-serif"   # --font-display: Latin wordmark, hero stats, eyebrows
    body: "'Pretendard Variable', -apple-system, BlinkMacSystemFont, 'Apple SD Gothic Neo', 'Noto Sans KR', system-ui, sans-serif"  # --font-body: Korean + body, headings
    mono: "'JetBrains Mono', ui-monospace, 'SFMono-Regular', 'Menlo', monospace"  # --font-mono: meta, tags, chips, nav, code
  scale:   # modular ~1.2, rem
    text-xs: 0.78rem     # eyebrows, meta, tags, chips, footer, kbd
    text-sm: 0.875rem    # captions, card desc, nav, TOC, tables
    text-base: 1rem      # body copy, card titles
    text-lg: 1.15rem     # h4, wordmark, hero lede, search input
    text-xl: 1.4rem      # h3, band name
    text-2xl: 1.75rem    # h2, hero stat values
    text-3xl: 2.2rem     # h1 / wiki + hero title
  leading:
    body: 1.7            # --leading-body — Korean readability
    tight: 1.25          # --leading-tight — headings
  notes:
    - "Headings use --font-body (Pretendard), weight 700, letter-spacing -0.01em, text-wrap: balance."
    - "Korean line-breaking: word-break: keep-all + overflow-wrap: anywhere on <body>."
    - "Hero/wiki title: clamp() responsive; Space Grotesk only for Latin display strings."

spacing:   # 4px base (--space-*)
  space-1: 0.25rem
  space-2: 0.5rem
  space-3: 0.75rem
  space-4: 1rem
  space-6: 1.5rem
  space-8: 2rem
  space-12: 3rem

rounded:
  radius-sm: 4px      # code, chips, kbd, focus-ring, skip target
  radius-md: 8px      # buttons, nav, filter chips, inputs, sheets, pre
  radius-lg: 14px     # cards, prev/next, search panel — the card radius

motion:
  dur: 0.18s
  ease: "cubic-bezier(0.4, 0, 0.2, 1)"
  note: "Everything is color/border/transform only. prefers-reduced-motion → ~0ms."

layout:
  measure: 72ch           # --measure — wiki reading column width
  header-h: 3.6rem        # --header-h — sticky header height (filter-bar offset anchor)
  content-max: 76rem      # home bands, filter bar, footer inner
  wiki-grid-max: 78rem    # wiki reading column + TOC rail
  hairline-w: 1px
  z-header: 100
  z-modal: 1000

components:
  site-header:
    background: "color-mix(in srgb, {colors.bg} 78%, transparent) + backdrop-blur(10px) saturate(160%)"
    borderBottom: "1px {colors.hairline}"
    position: "sticky top-0, z-header"
    note: "wordmark (ai·wiki, · dot = signal) + search(⌘K) + About + GitHub + theme toggle + mobile hamburger"
  hero:
    padding: "clamp(space-12, 12vh, 8rem) space-4"
    borderBottom: "1px {colors.hairline}"
    note: "hosts hero-constellation canvas behind hero-copy"
  hero-constellation:
    element: "<canvas>"
    opacity: 0.55
    mask: "radial-gradient(120% 90% at 70% 40%, #000 35%, transparent 88%)"
    note: "animated graph — golden-angle placement, ambient drift, signal edges (constellation.js)"
  hero-title:
    fontRole: body
    fontSize: "clamp(text-2xl, 6vw, text-3xl)"
    accentSpan: "{colors.signal}"     # .hero-accent
  home-filter:
    background: "color-mix(in srgb, {colors.bg} 82%, transparent) + backdrop-blur"
    position: "sticky top: {layout.header-h}, z: header-1"
    note: "category chips; mobile → horizontal scroll one row"
  filter-chip:
    fontRole: mono
    fontSize: "{typography.scale.text-sm}"
    background: "{colors.surface}"
    borderColor: "{colors.hairline}"
    rounded: "{rounded.radius-md}"
    hover: "border → {colors.signal}"
    active: "bg + border {colors.signal}, text {colors.bg}"   # .is-active — aqua fill
  card:
    background: "{colors.surface}"
    borderColor: "{colors.hairline}"
    rounded: "{rounded.radius-lg}"
    padding: "{spacing.space-4}"
    hover: "border → {colors.signal}, translateY(-2px)"
    parts: "card-tag (mono) · card-title (base/600) · card-desc (sm/muted, line-clamp 3) · card-foot (↳degree + chips)"
    neighborHover: "hovered card + linked cards keep signal border; unrelated dim to opacity 0.4"
  card-grid:
    columns: "1 → 2 (≥640px) → 3 (≥1024px)"
    gap: "{spacing.space-4}"
  reading-bar:
    element: "fixed top 2px bar, z: header+1"
    fill: "{colors.signal} (scaleX by reader.js on scroll)"
  wiki-grid:
    columns: "1 → [1fr, 15rem] at ≥1080px (reading column + TOC rail)"
    max: "{layout.wiki-grid-max}"
  wiki-toc:
    position: "sticky top 4.5rem (desktop ≥1080px only)"
    note: "scrollspy rail; active item = signal text + signal left-border. Mobile = <details> accordion."
  neigh:
    element: "radial SVG mini-graph (center = current page, ellipse of neighbor nodes)"
    edges: "{colors.signal-dim}; nodes {colors.faint} → {colors.signal} on hover; center = signal"
  src-links:
    fontRole: mono
    note: "원문(raw) · 요약 source · 이 페이지 .md → GitHub blob links"
  prevnext:
    columns: "2 → 1 (≤560px)"
    note: "same-category prev/next cards, radius-lg, hover signal border + lift"
  search-modal:
    background: "surface panel, backdrop = color-mix(bg 68%) + blur(4px)"
    rounded: "{rounded.radius-lg}"
    shadow: "0 24px 64px -24px rgba(0,0,0,0.6)"
    note: "Pagefind ⌘K modal; result highlight mark = signal-dim"
  site-footer:
    borderTop: "1px {colors.hairline}"
    fontRole: mono
    fontSize: "{typography.scale.text-xs}"
    note: "repo link · maintainer · 'built on Karpathy LLM Wiki pattern'"
---

## Overview

**ai-wiki** presents itself as a **deep-space knowledge-graph reader**. It is dark by default — a near-black ink canvas (`{colors.bg}` — #0b0e14) with layered surfaces — and the entire palette is monochrome except a single **luminous-aqua signal** (`{colors.signal}` — #5eead4). That one color does all the emphasis work: links, hover borders, the active filter chip's fill, the focus ring, the reading progress bar, and the animated constellation graph that runs behind the home hero.

The home page is a **card-grid feed** grouped into category bands, sitting under an animated `<canvas>` constellation hero. Each wiki page is a **reading experience**: a 72-character reading column, a sticky scrollspy table-of-contents rail on the right, a fixed reading-progress bar at the top, and — at the bottom — a small radial "neighborhood" graph of directly linked pages plus source links and prev/next navigation. Emphasis is a **color** here (aqua), not the absence of one.

The type system uses **three fonts by role**: Space Grotesk for Latin display (the `ai·wiki` wordmark, hero stat numbers, eyebrows), **Pretendard Variable** for Korean and body copy, and **JetBrains Mono** for the utility layer (meta labels, tags, chips, nav, code). Korean readability is deliberate — `word-break: keep-all`, a generous 1.7 body line-height, and `text-wrap: balance` on headings.

Technically this is a **hand-written static site**, not a framework build. Styles are plain CSS in one file, organized with `@layer tokens, base, components, utilities`; the tokens are CSS custom properties. HTML is generated by `build.mjs` from Markdown (marked + gray-matter + KaTeX + Pagefind) and deployed to GitHub Pages under the `/ai-wiki` base path.

**Key Characteristics:**

- **Dark-first, deep-space palette.** `:root` is the dark theme; light is an opt-in override (`:root[data-theme='light']`). Default resolves from `prefers-color-scheme`, set before paint by a FOUC-guard script and toggled by `theme.js`.
- **One signal accent.** Aqua `{colors.signal}` (#5eead4 dark / #0fb89b light) is the only chromatic value. It carries all emphasis; the rest of the system is ink and hairlines.
- **Three fonts, split by role** — Space Grotesk (display), Pretendard Variable (Korean/body), JetBrains Mono (mono/meta).
- **Signature interactions** — an animated constellation canvas hero and a card-grid feed with graph-**neighborhood hover** (a hovered card highlights its linked cards, the rest fade back).
- **Knowledge-graph reading page** — 72ch column + sticky TOC rail (scrollspy) + reading progress bar + bottom neighborhood mini-graph + source links + prev/next.
- **Hairline-and-frost depth.** Separation is a 1px `{colors.hairline}` border plus the surface step plus `backdrop-blur` frosted bars. Drop shadows appear only on floating overlays (search panel, mobile menu sheet, graph tooltip).
- **Plain CSS, `@layer`-ordered.** No Tailwind, no `tailwind.config`, no shadcn. Tokens live in `@layer tokens` inside [site/assets/css/styles.css](site/assets/css/styles.css).

## Colors

> **Source of truth:** [site/assets/css/styles.css](site/assets/css/styles.css) — `@layer tokens`, `:root` (dark, default) and `:root[data-theme='light']` (light override). Every value below is a CSS custom property; components consume them via `var(--…)`, never hardcoded hex.

### Token Map (Dark = default ↔ Light = override)

| Token                 | Dark (`:root`)          | Light (`[data-theme='light']`) | Use                                        |
| --------------------- | ----------------------- | ------------------------------ | ------------------------------------------ |
| `{colors.bg}`         | `#0b0e14`               | `#f7f8fa`                      | Page canvas (deep space ink ↔ near-white)  |
| `{colors.surface}`    | `#141923`               | `#ffffff`                      | Cards, header/menu, chips, buttons         |
| `{colors.surface-2}`  | `#1b2230`               | `#f1f3f7`                      | Code, tables `th`, `pre`, tooltip, chips   |
| `{colors.text}`       | `#e6e9ef`               | `#161a22`                      | Body / heading text                        |
| `{colors.muted}`      | `#8a93a3`               | `#5c6473`                      | Secondary text, meta, captions             |
| `{colors.faint}`      | `#5a6373`               | `#9aa2b0`                      | Tertiary — degree counts, list markers     |
| `{colors.hairline}`   | `#232a38`               | `#e3e7ee`                      | 1px borders everywhere (the depth grammar) |
| `{colors.signal}`     | `#5eead4`               | `#0fb89b`                      | **The accent** — emphasis, links, active   |
| `{colors.signal-dim}` | `rgba(94,234,212,.25)`  | `rgba(15,184,155,.18)`         | Underlines, edges, selection, glow         |

`color-scheme` is set per theme (`dark` / `light`) so native form controls and scrollbars match.

### The Signal Principle

- **Emphasis is aqua.** `{colors.signal}` is the one accent. It is used for: link text and (on hover) link underlines; card/chip/button hover borders; the **active** filter chip's *fill* (`background: signal; color: bg`); the focus-visible ring; the top reading-progress bar; the constellation hero nodes/edges; the neighborhood graph; and `::selection` (via `signal-dim`).
- **Everything else is monochrome.** Ink text on ink surfaces, separated by hairline borders and the surface step. No second hue is ever introduced.
- **No gradients as decoration.** Surfaces are flat. The only gradient in the system is the radial **mask** that fades the hero constellation canvas at its edges.

## Typography

### Three Fonts, by Role

| Role (`--font-*`) | Font                    | Used for                                                      |
| ----------------- | ----------------------- | ------------------------------------------------------------ |
| `display`         | **Space Grotesk**       | `ai·wiki` wordmark, hero stat numbers, some eyebrows          |
| `body`            | **Pretendard Variable** | Korean + Latin body copy, all headings (`h1`–`h6`)           |
| `mono`            | **JetBrains Mono**      | Meta labels, eyebrows, tags/chips, nav buttons, kbd, code     |

- **Pretendard Variable** is the workhorse — it covers Hangul + Latin in one face, so Korean and English body copy share metrics with no mixed-script seam. Loaded via a **dynamic-subset CSS** (`pretendard-dynamic-subset.css`), so only the glyphs a page uses are fetched.
- **Space Grotesk** and **JetBrains Mono** are self-hosted **Latin-subset woff2** (`font-display: swap`), used only for Latin display/utility strings. They are pulled from `@fontsource/*` and copied to `/static/fonts/` at build.

### Scale

| Token                    | Size    | Use                                                        |
| ------------------------ | ------- | ---------------------------------------------------------- |
| `{typography.scale.text-3xl}` | 2.2rem  | `h1` — wiki & hero title (via `clamp()`)              |
| `{typography.scale.text-2xl}` | 1.75rem | `h2`, hero stat values                                |
| `{typography.scale.text-xl}`  | 1.4rem  | `h3`, band (category) name                            |
| `{typography.scale.text-lg}`  | 1.15rem | `h4`, wordmark, hero lede, search input               |
| `{typography.scale.text-base}`| 1rem    | Body copy, card titles                                |
| `{typography.scale.text-sm}`  | 0.875rem| Captions, card desc, nav, TOC, tables                 |
| `{typography.scale.text-xs}`  | 0.78rem | Eyebrows, meta, tags, chips, footer, kbd              |

### Principles

- **Headings** use Pretendard at weight 700 (h3/h4 at 600), `letter-spacing: -0.01em`, `line-height: {typography.leading.tight}` (1.25), and `text-wrap: balance`.
- **Body** holds `{typography.scale.text-base}` at `line-height: {typography.leading.body}` (1.7) — tuned for Korean reading.
- **Korean line-breaking:** `word-break: keep-all` breaks on word boundaries; `overflow-wrap: anywhere` only forces a break on over-long tokens (URLs, IDs).
- **Mono is the utility voice** — anything that reads as a label, tag, count, or code path uses JetBrains Mono with slightly loosened tracking (`letter-spacing: 0.04–0.08em`, sometimes uppercased).

## Layout

### Containers

| Surface                     | Max width           |
| --------------------------- | ------------------- |
| Home bands / filter / footer| `76rem`             |
| Wiki page grid (col + rail) | `78rem`             |
| Wiki reading column         | `{layout.measure}` (72ch) |
| Interim `.shell` reading    | `72ch`              |

All centered with `margin: 0 auto`, padded on the `{spacing.space-4}` gutter.

### App Shell (home)

Top to bottom: **sticky frosted header** → **constellation hero** → **sticky category filter bar** → **category bands**, each a header (name · count · description) over a **card grid** (1 → 2 → 3 columns) → **footer**. The filter bar sticks at `top: {layout.header-h}` so it rides just under the header; bands carry `scroll-margin-top` so anchor jumps clear both sticky bars.

### Wiki Page

A CSS grid: single column on mobile, `[minmax(0,1fr), 15rem]` at ≥1080px (reading column + TOC rail). Above it, a fixed 2px **reading-progress bar**. The article is the 72ch `.prose` column with a page header (category eyebrow · title · meta), then the rendered body, then a footer block: **관련 페이지** (neighborhood SVG + list) → **원문 · 소스** (GitHub links) → prev/next.

### Whitespace

The 4px spacing scale (`{spacing.space-1}`…`{spacing.space-12}`) drives everything: card padding `{spacing.space-4}`, grid gaps `{spacing.space-4}`, section rhythm `{spacing.space-8}`–`{spacing.space-12}`, chip gaps `{spacing.space-2}`.

## Signature Interactions

These are what make the site feel like a *graph*, not a list — all in vanilla JS ([site/assets/js/](site/assets/js/)), all reduced-motion aware.

- **Constellation hero** ([constellation.js](site/assets/js/constellation.js)) — a `<canvas>` renders `graph.json`: nodes placed by **golden-angle** distribution (deterministic, stable across reloads), drifting on a bounded ambient path, connected by `signal-dim` edges; higher-degree nodes are larger and aqua. Colors are read live from CSS tokens, so it follows the theme. `prefers-reduced-motion` → a single static frame.
- **Card neighborhood hover** — hovering a card adds `cards-focusing`: the card and its graph-linked cards keep an aqua border while unrelated cards drop to `opacity: 0.4`. The links come from `data-links` on each card (adjacency baked in at build).
- **TOC scrollspy** ([reader.js](site/assets/js/reader.js)) — the sticky rail's active item flips to aqua text + aqua left-border as you scroll; also drives the top reading-progress bar (`scaleX`) and the neighborhood-graph node ↔ list-row hover pairing.
- **⌘K search** ([search.js](site/assets/js/search.js)) — a Pagefind-backed modal (frosted backdrop, surface panel), keyboard-navigable, with `signal-dim` highlight marks on matched excerpts.

## Elevation & Depth

| Level        | Treatment                                            | Use                                            |
| ------------ | ---------------------------------------------------- | ---------------------------------------------- |
| Flat         | No border, no shadow                                 | Page canvas, prose text                        |
| Hairline     | 1px `{colors.hairline}`                               | Cards, inputs, tables, chips, sheet edges, TOC |
| Surface step | `{colors.surface}` / `{colors.surface-2}` over `bg`  | Cards, code, tooltips — separation by lightness |
| Frosted      | `color-mix(bg 78–82%)` + `backdrop-blur(10px)`       | Header, filter bar, search backdrop            |
| Overlay shadow | `0 …px … rgba(0,0,0,.5–.6)`                         | Search panel, mobile menu sheet, graph tooltip |

**Shadow philosophy — restrained, not banned.** Cards, inputs, and bars get *no* shadow; their depth is the hairline border + surface step + frost. Drop shadows are reserved for genuinely floating overlays (the ⌘K search panel, the mobile hamburger menu, the neighborhood-node tooltip) where the element must read as lifted above everything.

## Shapes

### Border Radius Scale

| Token                  | Value | Use                                                     |
| ---------------------- | ----- | ------------------------------------------------------- |
| `{rounded.radius-sm}`  | 4px   | Inline code, chips, kbd, focus ring, skip-link target   |
| `{rounded.radius-md}`  | 8px   | Buttons, nav, filter chips, inputs, `pre`, mobile sheet  |
| `{rounded.radius-lg}`  | 14px  | Cards, prev/next cards, search panel — the card radius   |

Corners are consistent by role: interactive utility bits at 8px, cards and panels at 14px, tiny inline bits at 4px. There is no pill/capsule grammar — chips are small rounded rectangles (`radius-md`/`radius-sm`), not full pills.

## Light / Dark Theme

- **Mechanism.** Theme is a `data-theme` attribute on `<html>`. A tiny inline **FOUC-guard script** runs before paint: it reads `localStorage.theme`, else falls back to `prefers-color-scheme`, and sets `data-theme` so the first paint is already correct. [theme.js](site/assets/js/theme.js) handles the toggle button and persistence.
- **Default is dark.** `:root` *is* the dark theme; `:root[data-theme='light']` overrides the same variable names with light values. Every color resolves through `var(--…)`, so no component needs theme-specific markup.
- **Everything follows.** Because the constellation canvas reads its colors from CSS tokens at runtime, even the animated graph re-tints on theme switch.
- **Browser chrome.** OG/canonical absolute URLs and the dark-default `color-scheme` keep native UI in sync.

## Responsive Behavior

| Width    | Layout change                                                          |
| -------- | --------------------------------------------------------------------- |
| ≤560px   | Mobile nav: hide inline About/GitHub, show hamburger + `#site-menu` sheet; nav labels shrink to icons; filter bar becomes a single horizontal-scroll row; prev/next stacks to 1 column |
| ≥640px   | Card grid → 2 columns                                                  |
| ≥1024px  | Card grid → 3 columns                                                  |
| ≥1080px  | Wiki page gains the right **sticky TOC rail**; the mobile `<details>` TOC hides |

The hero title and wiki/hero headings scale fluidly with `clamp()`; most other type holds its size and reflows by container width.

### Reduced Motion

`@media (prefers-reduced-motion: reduce)` drops all transition/animation durations to ~0ms and disables smooth scroll. Safe, because every animation is decorative (color, border, transform, the hero drift) — never load-bearing. The constellation canvas honors it explicitly by drawing one static frame.

## Do's and Don'ts

### Do

- Carry emphasis with the **aqua signal** — link color, hover borders, the active chip fill, the focus ring, the reading bar. It is the one accent.
- Keep every other surface monochrome ink + hairlines. Separate by border and surface step, not by a new hue.
- Use the **right font for the role**: Space Grotesk for Latin display, Pretendard for Korean/body, JetBrains Mono for meta/tags/code.
- Bound surfaces with the 1px `{colors.hairline}` border; frost floating bars with `backdrop-blur`.
- Reserve drop shadows for true overlays (search panel, mobile menu, graph tooltip) — not cards or bars.
- Respect Korean typography: `word-break: keep-all`, 1.7 body leading, `text-wrap: balance` on headings.
- Define every new color as a **dark (`:root`) + light (`[data-theme='light']`) pair** under the same variable name.
- Reach for the token custom properties (`var(--signal)`, `var(--hairline)`, `var(--space-4)`, …) — never inline hex or magic numbers.

### Don't

- Don't introduce a second accent color — aqua is the whole emphasis system.
- Don't add shadows to cards, inputs, chips, or the sticky bars; they're hairline + frost by design.
- Don't use gradients decoratively (the only gradient is the hero canvas edge **mask**).
- Don't mix up the font roles — Korean text must be Pretendard; don't set body copy in Space Grotesk or Mono.
- Don't hardcode hex or px in the templates; consume the CSS tokens so theme + spacing stay consistent.
- Don't assume a light default — the site is dark-first; author and test dark first, then verify the light override.
- Don't let sticky chrome overlap anchored content — keep the `scroll-margin-top` offsets for header + filter bar.

## Iteration Guide

1. Edit tokens in [site/assets/css/styles.css](site/assets/css/styles.css) `@layer tokens` (`:root` for dark, `:root[data-theme='light']` for light). This file is the single source of truth; this document mirrors it.
2. Always define a color as a **dark/light pair** under the same variable name.
3. New components go in `@layer components`; utilities in `@layer utilities`. The `@layer` order (`tokens, base, components, utilities`) is what prevents specificity fights — respect it.
4. Reference token variables and existing class patterns — never inline hex.
5. Keep the three font roles distinct; don't add a fourth face.
6. Depth is hairline border + surface step + frost. Add an overlay shadow only if the element genuinely floats above the page.
7. HTML structure lives in [site/lib/templates.mjs](site/lib/templates.mjs); interactions in [site/assets/js/](site/assets/js/). Rebuild with `node build.mjs` (from `site/`) and preview with `npm run preview`.

## Known Gaps

- **No formal spacing/type utility classes** — components use the CSS variables directly; there is no atomic utility layer (`@layer utilities` is declared but lightly used).
- **A few literal values remain** — hero mask percentages, `clamp()` bounds, and overlay shadow blurs are literals rather than named tokens.
- **`backdrop-blur` radius** is a fixed `10px` (bars) / `4px` (search) literal, not a token.
- **Type scale is documented but not all steps are heavily used** — e.g. `text-lg` appears in only a few places.
- **Light theme is a full override but secondary** — the site is authored dark-first; a few contrast edge cases in light mode (faint-on-white meta) are acceptable-but-not-tuned.
- **Latin fonts are Latin-subset only** — Space Grotesk / JetBrains Mono have no Hangul; any Korean accidentally set in those roles falls back to the body stack.
