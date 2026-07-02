---
version: 2.1
name: starter-kit-design
description: A deep-space, dark-first design system for the INU lecture static starter kit. The whole system is monochrome ink except one luminous-aqua signal color (#5eead4) that carries every bit of emphasis — links, hover borders, the active filter chip, the focus ring, the primary button fill. Three fonts split by role: Space Grotesk for Latin display (wordmark, eyebrows), Pretendard for Korean + body + headings, JetBrains Mono for meta/tags/chips/nav/code. It is hand-written CSS (no Tailwind, no shadcn, no build step) split across css/tokens.css → base.css → components.css; tokens are CSS custom properties. Rendered as a pure HTML/CSS/JS static site deployed to GitHub Pages (main/root, no build), themed dark by default with an opt-in light override via `data-theme` on `<html>`.

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
    display: "'Space Grotesk', ui-sans-serif, system-ui, sans-serif"   # --font-display: Latin wordmark, eyebrows
    body: "'Pretendard Variable', 'Pretendard', -apple-system, BlinkMacSystemFont, 'Apple SD Gothic Neo', 'Noto Sans KR', system-ui, sans-serif"  # --font-body: Korean + body, headings
    mono: "'JetBrains Mono', ui-monospace, 'SFMono-Regular', 'Menlo', monospace"  # --font-mono: meta, tags, chips, nav, code
  scale:   # modular ~1.2, rem
    text-xs: 0.78rem     # eyebrows, meta, tags, chips, footer, kbd
    text-sm: 0.875rem    # captions, card desc, nav, search input, tables
    text-base: 1rem      # body copy, card titles
    text-lg: 1.15rem     # h4, wordmark, hero lede
    text-xl: 1.4rem      # h3, sub-nav title
    text-2xl: 1.75rem    # h2
    text-3xl: 2.2rem     # h1 / hero title
  leading:
    body: 1.7            # --leading-body — Korean readability
    tight: 1.25          # --leading-tight — headings
  notes:
    - "Headings use --font-body (Pretendard), weight 700 (600 for lighter headings), letter-spacing -0.01em, text-wrap: balance."
    - "Korean line-breaking: word-break: keep-all + overflow-wrap: anywhere on <body>."
    - "Hero title uses clamp() responsive sizing; Space Grotesk only for Latin display strings (wordmark, eyebrows)."

spacing:   # 4px base (--space-*)
  space-1: 0.25rem
  space-2: 0.5rem
  space-3: 0.75rem
  space-4: 1rem
  space-6: 1.5rem
  space-8: 2rem
  space-12: 3rem

rounded:
  radius-sm: 4px      # inline code, chips-inline, kbd, focus-ring
  radius-md: 8px      # buttons, nav links, filter chips, inputs, mobile sheet
  radius-lg: 14px     # cards — the card radius

motion:
  dur: 0.18s
  ease: "cubic-bezier(0.4, 0, 0.2, 1)"
  note: "Everything is color/border/transform only. prefers-reduced-motion → ~0ms."

layout:
  measure: 72ch           # --measure — text-page reading column width (about.html)
  header-h: 3.6rem        # --header-h — sticky header height (sub-nav offset anchor)
  content-max: 76rem      # --content-max — home/grid/footer inner width
  hairline-w: 1px
  z-header: 100

components:
  site-header:
    class: ".global-nav (js/layout.js 가 주입)"
    background: "color-mix(in srgb, {colors.bg} 78%, transparent) + backdrop-blur(10px) saturate(160%)"
    borderBottom: "1px {colors.hairline}"
    position: "sticky top-0, z-header"
    note: "wordmark (inu·starter, · dot = signal) + nav links(mono) + theme toggle button + mobile hamburger. No search."
  hero:
    class: ".hero (index.html)"
    padding: "clamp(space-12, 12vh, 8rem) space-4"
    borderBottom: "1px {colors.hairline}"
    note: "static deep-space hero — no constellation canvas. eyebrow + title + lede + CTA row."
  hero-accent:
    class: ".hero-accent"
    color: "{colors.signal}"     # one emphasized word inside the hero title
  filter-chip:
    class: ".chip / .chip.is-active"
    fontRole: mono
    fontSize: "{typography.scale.text-sm}"
    background: "{colors.surface}"
    borderColor: "{colors.hairline}"
    rounded: "{rounded.radius-md}"
    hover: "border → {colors.signal}"
    active: "bg + border {colors.signal}, text {colors.bg}"   # .is-active — aqua fill
  button:
    class: ".btn / .btn--primary / .btn--ghost / .btn--lg"
    background: "{colors.surface} + 1px {colors.hairline}"
    rounded: "{rounded.radius-md}"
    hover: "border → {colors.signal}"
    active: "transform: scale(0.95)"
    primary: ".btn--primary = aqua fill (bg {colors.signal}, text {colors.bg}) — the single strong CTA"
    ghost: ".btn--ghost = transparent bg, signal text"
  card:
    class: ".card"
    background: "{colors.surface}"
    borderColor: "{colors.hairline}"
    rounded: "{rounded.radius-lg}"
    padding: "{spacing.space-4}"
    hover: "border → {colors.signal}, translateY(-2px)"
    parts: "card-tag (mono) · card-title (base/600) · card-desc (sm/muted, line-clamp 3) · card-foot (mono/faint)"
  card-grid:
    class: ".card-grid"
    columns: "1 → 2 (≥640px) → 3 (≥1024px)"
    gap: "{spacing.space-4}"
  search-input:
    class: ".search-input"
    background: "{colors.surface}"
    borderColor: "{colors.hairline}"
    rounded: "{rounded.radius-md}"
    focus: "border → {colors.signal} (outline none)"
    note: "showcase input only — not a search modal."
  sub-nav:
    class: ".sub-nav (components.html)"
    background: "color-mix(in srgb, {colors.bg} 82%, transparent) + backdrop-blur"
    position: "sticky top: {layout.header-h}, z: header-1"
    note: "frosted category strip: title + in-page anchor links + a primary button."
  site-footer:
    class: ".footer (js/layout.js 가 주입)"
    borderTop: "1px {colors.hairline}"
    fontRole: mono
    fontSize: "{typography.scale.text-xs}"
    note: "column links (프로젝트 / 리소스) + legal line."
---

## Overview

The **INU lecture starter kit** presents itself as a **deep-space, dark-first** static site. It is dark by default — a near-black ink canvas (`{colors.bg}` — #0b0e14) with layered surfaces — and the entire palette is monochrome except a single **luminous-aqua signal** (`{colors.signal}` — #5eead4). That one color does all the emphasis work: links, hover borders, the active filter chip's fill, the primary button fill, and the focus ring.

The site is **three independent HTML pages**: `index.html` (a static hero + a filter-chip and card-grid demo feed), `about.html` (a text-centric reading page in a 72ch column), and `components.html` (a component showcase). A common frosted sticky header and footer are injected into each page by `js/layout.js` (there is no build step, so JS template strings replace a server-side include).

The type system uses **three fonts by role**: Space Grotesk for Latin display (the `inu·wiki`-style wordmark, eyebrows), **Pretendard** for Korean and body copy and all headings, and **JetBrains Mono** for the utility layer (meta labels, tags, chips, nav, code). Korean readability is deliberate — `word-break: keep-all`, a generous 1.7 body line-height, and `text-wrap: balance` on headings.

Technically this is a **hand-written static site**, not a framework build. Styles are plain CSS split across three files loaded in order — [css/tokens.css](css/tokens.css) → [css/base.css](css/base.css) → [css/components.css](css/components.css) — where the tokens are CSS custom properties. There is **no npm, no Node, no build/bundle step**; the repo is deployed to GitHub Pages by pointing Pages at `main`/root.

**Key Characteristics:**

- **Dark-first, deep-space palette.** `:root` is the dark theme; light is an opt-in override (`:root[data-theme='light']`). The first paint is set by a FOUC-guard inline script (reads `localStorage.theme`, else `prefers-color-scheme`) and toggled by the header button.
- **One signal accent.** Aqua `{colors.signal}` (#5eead4 dark / #0fb89b light) is the only chromatic value. It carries all emphasis; the rest of the system is ink and hairlines.
- **Three fonts, split by role** — Space Grotesk (display), Pretendard (Korean/body/headings), JetBrains Mono (mono/meta).
- **Card-grid feed** — the home page groups a filter-chip row over a responsive card grid (1 → 2 → 3 columns), each card a hairline-bordered surface that lifts and takes a signal border on hover.
- **Hairline-and-frost depth.** Separation is a 1px `{colors.hairline}` border plus the surface step plus `backdrop-blur` frosted bars. Drop shadows appear only on the floating mobile menu sheet.
- **Plain CSS, no build.** No Tailwind, no `tailwind.config`, no shadcn, no `@layer`. Tokens live in [css/tokens.css](css/tokens.css); interactions are vanilla JS on the `window.INU` namespace.

## Colors

> **Source of truth:** [css/tokens.css](css/tokens.css) — `:root` (dark, default) and `:root[data-theme='light']` (light override). Every value below is a CSS custom property (`--color-…`); components consume them via `var(--…)`, never hardcoded hex.

### Token Map (Dark = default ↔ Light = override)

| Token                 | Dark (`:root`)          | Light (`[data-theme='light']`) | Use                                        |
| --------------------- | ----------------------- | ------------------------------ | ------------------------------------------ |
| `{colors.bg}`         | `#0b0e14`               | `#f7f8fa`                      | Page canvas (deep space ink ↔ near-white)  |
| `{colors.surface}`    | `#141923`               | `#ffffff`                      | Cards, header/menu, chips, buttons         |
| `{colors.surface-2}`  | `#1b2230`               | `#f1f3f7`                      | Inline code, tooltips                      |
| `{colors.text}`       | `#e6e9ef`               | `#161a22`                      | Body / heading text                        |
| `{colors.muted}`      | `#8a93a3`               | `#5c6473`                      | Secondary text, meta, captions             |
| `{colors.faint}`      | `#5a6373`               | `#9aa2b0`                      | Tertiary — card foot, list markers         |
| `{colors.hairline}`   | `#232a38`               | `#e3e7ee`                      | 1px borders everywhere (the depth grammar) |
| `{colors.signal}`     | `#5eead4`               | `#0fb89b`                      | **The accent** — emphasis, links, active   |
| `{colors.signal-dim}` | `rgba(94,234,212,.25)`  | `rgba(15,184,155,.18)`         | Underlines, selection, glow                |

`color-scheme` is set per theme (`dark` / `light`) so native form controls and scrollbars match.

### The Signal Principle

- **Emphasis is aqua.** `{colors.signal}` is the one accent. It is used for: link text and (on hover) link underlines; card/chip/button hover borders; the **active** filter chip's *fill* and the **primary** button's *fill* (`background: signal; color: bg`); the focus-visible ring; and `::selection` (via `signal-dim`).
- **Everything else is monochrome.** Ink text on ink surfaces, separated by hairline borders and the surface step. No second hue is ever introduced.
- **No gradients as decoration.** Surfaces are flat.

## Typography

### Three Fonts, by Role

| Role (`--font-*`) | Font                    | Used for                                                      |
| ----------------- | ----------------------- | ------------------------------------------------------------ |
| `display`         | **Space Grotesk**       | `inu·starter` wordmark, some eyebrows                         |
| `body`            | **Pretendard**          | Korean + Latin body copy, all headings (`h1`–`h6`)           |
| `mono`            | **JetBrains Mono**      | Meta labels, eyebrows, tags/chips, nav buttons, kbd, code     |

- **Pretendard** is the workhorse — it covers Hangul + Latin in one face, so Korean and English body copy share metrics with no mixed-script seam. Loaded via the jsDelivr **dynamic-subset CSS** (`pretendardvariable-dynamic-subset.css`), so only the glyphs a page uses are fetched.
- **Space Grotesk** and **JetBrains Mono** are loaded from **Google Fonts** (`<link>` in each page `<head>`, `display=swap`), used only for Latin display/utility strings. They carry no Hangul; any Korean set in those roles falls back to the body stack.

### Scale

| Token                    | Size    | Use                                                        |
| ------------------------ | ------- | ---------------------------------------------------------- |
| `{typography.scale.text-3xl}` | 2.2rem  | `h1` — hero title (via `clamp()`)                     |
| `{typography.scale.text-2xl}` | 1.75rem | `h2` / section headings                               |
| `{typography.scale.text-xl}`  | 1.4rem  | `h3` / sub-nav title                                  |
| `{typography.scale.text-lg}`  | 1.15rem | `h4`, wordmark, hero lede                             |
| `{typography.scale.text-base}`| 1rem    | Body copy, card titles                                |
| `{typography.scale.text-sm}`  | 0.875rem| Captions, card desc, nav, search input                |
| `{typography.scale.text-xs}`  | 0.78rem | Eyebrows, meta, tags, chips, footer, kbd              |

The `.t-*` utility classes in [css/base.css](css/base.css) apply these steps together with the right font role (`.t-hero-display`, `.t-display-lg`, `.t-display-md`, `.t-tagline`, `.t-lead`, `.t-body`, `.t-caption`, `.t-eyebrow`, …).

### Principles

- **Headings** use Pretendard at weight 700 (600 for lighter roles), `letter-spacing: -0.01em`, `line-height: {typography.leading.tight}` (1.25), and `text-wrap: balance`.
- **Body** holds `{typography.scale.text-base}` at `line-height: {typography.leading.body}` (1.7) — tuned for Korean reading.
- **Korean line-breaking:** `word-break: keep-all` breaks on word boundaries; `overflow-wrap: anywhere` only forces a break on over-long tokens (URLs, IDs).
- **Mono is the utility voice** — anything that reads as a label, tag, count, or code path uses JetBrains Mono with slightly loosened tracking (`letter-spacing: 0.04–0.08em`, sometimes uppercased).

## Layout

### Containers

| Surface                       | Max width                 | Class            |
| ----------------------------- | ------------------------- | ---------------- |
| Home feed / components / footer | `{layout.content-max}` (76rem) | `.container-grid` |
| Text page reading column      | `{layout.measure}` (72ch) | `.container-text` |

Both centered with `margin-inline: auto`, padded on the `{spacing.space-4}` gutter.

### Page Shells

- **index.html** — sticky frosted header → static hero (eyebrow · title · lede · CTA) → a `.container-grid` section with a filter-chip row over a card grid (1 → 2 → 3 columns) → footer.
- **about.html** — header → a 72ch `.container-text` reading column (eyebrow · h1 · body · sub-headings) → footer.
- **components.html** — header → frosted `.sub-nav` strip → `.container-grid` of showcase sections (typography, buttons, chips, input, cards) → footer.

Header and footer are injected on every page by [js/layout.js](js/layout.js) into the `#site-nav` / `#site-footer` placeholders.

### Whitespace

The 4px spacing scale (`{spacing.space-1}`…`{spacing.space-12}`) drives everything: card padding `{spacing.space-4}`, grid gaps `{spacing.space-4}`, section rhythm `{spacing.space-8}`–`{spacing.space-12}`, chip gaps `{spacing.space-2}`.

## Signature Interactions

All in vanilla JS on the `window.INU` namespace ([js/main.js](js/main.js)), all reduced-motion aware.

- **Theme toggle** ([js/main.js](js/main.js) `bindTheme`) — the header button flips `data-theme` on `<html>` between dark and light and persists the choice to `localStorage`. The very first paint is decided before load by a small FOUC-guard inline script in each page `<head>`.
- **Mobile menu** ([js/main.js](js/main.js) `bindNav`) — below 560px the nav collapses behind a hamburger; the toggle opens a surface-fill sheet (the one place a drop shadow is used) and syncs `aria-expanded`.
- **Card hover** — a card takes a signal border and lifts `translateY(-2px)` on hover (CSS only).
- **Chip active** — the active filter chip fills with the aqua signal (`.is-active`); hover gives a signal border.

## Elevation & Depth

| Level        | Treatment                                            | Use                                            |
| ------------ | ---------------------------------------------------- | ---------------------------------------------- |
| Flat         | No border, no shadow                                 | Page canvas, prose text                        |
| Hairline     | 1px `{colors.hairline}`                               | Cards, inputs, chips, buttons, sub-nav         |
| Surface step | `{colors.surface}` / `{colors.surface-2}` over `bg`  | Cards, code, tooltips — separation by lightness |
| Frosted      | `color-mix(bg 78–82%)` + `backdrop-blur(10px)`       | Header, sub-nav                                |
| Overlay shadow | `0 24px 48px -24px rgba(0,0,0,.6)`                 | Mobile menu sheet only                         |

**Shadow philosophy — restrained, not banned.** Cards, inputs, chips, and the sticky bars get *no* shadow; their depth is the hairline border + surface step + frost. A drop shadow is reserved for the genuinely floating mobile hamburger sheet.

## Shapes

### Border Radius Scale

| Token                  | Value | Use                                                     |
| ---------------------- | ----- | ------------------------------------------------------- |
| `{rounded.radius-sm}`  | 4px   | Inline code, kbd, focus ring                            |
| `{rounded.radius-md}`  | 8px   | Buttons, nav links, filter chips, inputs, mobile sheet  |
| `{rounded.radius-lg}`  | 14px  | Cards — the card radius                                 |

Corners are consistent by role: interactive utility bits at 8px, cards at 14px, tiny inline bits at 4px. There is no pill/capsule grammar — chips and buttons are small rounded rectangles (`radius-md`), not full pills.

## Light / Dark Theme

- **Mechanism.** Theme is a `data-theme` attribute on `<html>`. A tiny inline **FOUC-guard script** in each page `<head>` runs before paint: it reads `localStorage.theme`, else falls back to `prefers-color-scheme`, and sets `data-theme` so the first paint is already correct. [js/main.js](js/main.js) `bindTheme()` handles the toggle button and persistence.
- **Default is dark.** `:root` *is* the dark theme; `:root[data-theme='light']` overrides the same variable names with light values. Every color resolves through `var(--…)`, so no component needs theme-specific markup.
- **Browser chrome.** `color-scheme` per theme keeps native form controls and scrollbars in sync.

## Responsive Behavior

| Width    | Layout change                                                          |
| -------- | --------------------------------------------------------------------- |
| ≤560px   | Mobile nav: hide inline links, show hamburger + surface sheet; filter chip row becomes a horizontal-scroll row; footer columns stack to 1 |
| ≥640px   | Card grid → 2 columns                                                  |
| ≥1024px  | Card grid → 3 columns                                                  |

The hero title scales fluidly with `clamp()`; most other type holds its size and reflows by container width.

### Reduced Motion

`@media (prefers-reduced-motion: reduce)` drops all transition/animation durations to ~0ms and disables smooth scroll. Safe, because every animation is decorative (color, border, transform) — never load-bearing.

## Do's and Don'ts

### Do

- Carry emphasis with the **aqua signal** — link color, hover borders, the active chip fill, the primary button fill, the focus ring. It is the one accent.
- Keep every other surface monochrome ink + hairlines. Separate by border and surface step, not by a new hue.
- Use the **right font for the role**: Space Grotesk for Latin display, Pretendard for Korean/body, JetBrains Mono for meta/tags/code.
- Bound surfaces with the 1px `{colors.hairline}` border; frost the sticky bars with `backdrop-blur`.
- Reserve the drop shadow for the mobile menu sheet — not cards, inputs, or bars.
- Respect Korean typography: `word-break: keep-all`, 1.7 body leading, `text-wrap: balance` on headings.
- Define every new color as a **dark (`:root`) + light (`[data-theme='light']`) pair** under the same variable name.
- Reach for the token custom properties (`var(--color-signal)`, `var(--color-hairline)`, `var(--space-4)`, …) — never inline hex or magic numbers.
- Keep paths **relative** (no leading `/`) and add nav items via `NAV_LINKS` in [js/layout.js](js/layout.js).

### Don't

- Don't introduce a second accent color — aqua is the whole emphasis system.
- Don't add shadows to cards, inputs, chips, or the sticky bars; they're hairline + frost by design.
- Don't use gradients decoratively.
- Don't mix up the font roles — Korean text must be Pretendard; don't set body copy in Space Grotesk or Mono.
- Don't hardcode hex or px in the pages; consume the CSS tokens so theme + spacing stay consistent.
- Don't assume a light default — the site is dark-first; author and test dark first, then verify the light override.
- Don't add a build step, npm dependency, or bundler — the kit is pure HTML/CSS/JS by design.

## Iteration Guide

1. Edit tokens in [css/tokens.css](css/tokens.css) (`:root` for dark, `:root[data-theme='light']` for light). This file is the single source of truth; this document mirrors it.
2. Always define a color as a **dark/light pair** under the same variable name.
3. Base resets + typography utilities live in [css/base.css](css/base.css); component classes in [css/components.css](css/components.css). The three files load in order (`tokens → base → components`) — later files override earlier ones by source order (there is no `@layer`).
4. Reference token variables and existing class patterns — never inline hex.
5. Keep the three font roles distinct; don't add a fourth face.
6. Depth is hairline border + surface step + frost. Add the overlay shadow only for the mobile menu sheet.
7. Common layout (nav/footer) is generated in [js/layout.js](js/layout.js); interactions in [js/main.js](js/main.js). Preview locally with `python3 -m http.server 8000` (or just open the HTML files); deploy by pointing GitHub Pages at `main`/root — no build.

## Known Gaps

- **No formal spacing/type utility layer beyond `.t-*`** — components reference the CSS variables directly; there is no atomic utility system.
- **A few literal values remain** — hero padding `clamp()` bounds and the mobile-sheet shadow blur are literals rather than named tokens.
- **`backdrop-blur` radius** is a fixed `10px` literal, not a token.
- **Light theme is a full override but secondary** — the site is authored dark-first; a few contrast edge cases in light mode (faint-on-white meta) are acceptable-but-not-tuned.
- **Latin fonts are Latin-only** — Space Grotesk / JetBrains Mono have no Hangul; any Korean accidentally set in those roles falls back to the body (Pretendard) stack.
- **Common layout depends on JS** — nav/footer are injected by [js/layout.js](js/layout.js); with JS disabled the `#site-nav` / `#site-footer` placeholders render empty.
