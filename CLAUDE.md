# The Green House Foundation — website

Static marketing site for **The Green House Foundation**, a Utah 501(c)(3) that
preserves culturally significant art, develops artists into sustainable careers,
and builds cultural spaces across the Mountain West. Home of The Green House
Gallery in historic downtown Huntsville, Utah, and the Bacone collection.

Plain hand-written HTML + CSS. **There is no build step, no framework, no npm,
no bundler** — you preview a page by opening its `.html` file in a browser. Do
not add a build tool, framework, or dependency without asking me first.

---

## Files

| File | What it is |
|------|-----------|
| `index.html` | Foundation homepage. The big one (~700 lines): mission, Bacone collection, BEACON, the corridor, endowment, get involved. |
| `about.html` | About page — the valley, the three communities, why the Foundation exists. |
| `grants.html` | Grants being sought: the $5M 2026 goal, fundable priorities, grant sources. |
| `hero-loop.html` | **Standalone ambient hero animation** (CSS god-rays / breathing gradient). Self-contained art piece with its own palette — leave its inline `:root` alone. |
| `styles.css` + `tokens/*.css` | A design-token layer (palette, type scale, spacing, theme scopes). **The pages don't actually consume it** — see below. |
| `assets/logos/` | Brand logo PNGs (also base64-inlined into each page's nav/footer). |

No tests, no CI. If you add tooling, document it here.

---

## How the pages are actually built

Each page is **self-contained**. It links `styles.css`, but then defines its own
inline `<style>` block with its own palette vars (`--ink`, `--paper`, `--clay`,
`--ember`, …) and its own component classes (`.wrap`, `.btn`, `.eyebrow`,
`header.nav`, `.ft`). The three pages carry an **identical inline `:root`**
palette (copied verbatim), so the real styling lives in each file, not in the
token layer. `tokens/*.css` is a parallel system the pages do not use.

When you edit a page, work **within the inline vars already defined at the top of
that file**. Don't wire the pages up to the token layer unless we decide to do
that on purpose.

Two rules keep edits clean:

1. **Don't introduce a new color.** Use a value that already exists — an inline
   var on that page, or one of the palette values below.
2. **Don't hardcode a hex/px that already has a name.** If `--ember-deep` exists,
   write `var(--ember-deep)`, not `#A85C2F`.

---

## Palette — as the site actually uses it

A warm, earthy system on true-black ink and white/warm-paper grounds. These vars
are defined identically at the top of all three pages:

| Token | Value | Use |
|-------|-------|-----|
| `--ink` / `--ink-2` | `#0C0C0A` / `#45493B` | primary text; secondary text + dark section grounds |
| `--paper` / `--paper-2` | `#FFFFFF` / `#F6F6EF` | page ground; warm off-white panels |
| `--olive` / `--olive-deep` | `#858549` / `#6F6F3D` | **brand** — links, brand marks, primary buttons |
| `--clay` | `#C2704F` | **accent** — eyebrows, nav underline, focus rings, link hovers |
| `--sage` | `#A3A36B` | section/step numbers, labels, badges |
| `--ochre` | `#DAB877` | display emphasis (italic highlights) |
| `--ember` / `--ember-deep` | `#C2703D` / `#A85C2F` | **money & emphasis** — the $5M figure, endowment pulse, required-field marks, and primary buttons |
| `--cream` | `#F4F5E7` | light text on dark grounds |
| `--ground` | `#F0E7D6` | warm storytelling ground |
| `--hair` | `rgba(26,30,20,0.12)` | hairline borders |

**clay / sage / ochre / ember are the site's real accent voice** and appear on
every page — that is intentional; keep them consistent across the three pages.
(`tokens/bacone.css` scopes a similar-looking palette to `.theme-bacone` for
storytelling blocks; that token file is separate and unrelated to the inline page
palettes above.)

## Type & spacing
- **Display / headings:** Newsreader (serif). **Body / labels:** Inter. Both are
  loaded per page (see conventions) with Georgia / system fallbacks inline.
- Roughly golden-ratio type scale; eyebrows 12px, uppercase, ~`0.18em` tracking.
- 8px spacing rhythm. Radii are architectural, not pill-soft (pills are for
  chips/toggles only). Motion is calm — no bounce.

---

## Shared components are duplicated per page — edit them together
The **nav** (`header.nav`, sticky, brand + logo + `.nav-links` + a `.btn` CTA), the
**footer** (`.ft`, Explore / Visit & Contact columns + legal line), the **buttons**
(`.btn`, `.btn-olive`, `.btn-cream`, `.btn-cream-ghost`, `.btn-sm`), and the
`.wrap` container are copy-pasted into `index.html`, `about.html`, and
`grants.html`. **If you change one, change all three,** or they drift. The nav's
`active` link state is set per page.

## Conventions to preserve
- **`@dsCard` comment on line 1** of each page (`group`, `viewport`, `name`,
  `subtitle`) — design-gallery metadata. Keep it; update `name`/`subtitle` when
  you add a page.
- **Fonts load twice on purpose:** once via `styles.css`'s `@import`, and again as
  a `<link>` in each page's `<head>` (with `preconnect`) so the page renders fast
  before the stylesheet parses. Keep the `<head>` link.
- **Logos are base64-inlined** into the nav and footer so a page renders even if
  `assets/` doesn't load. Copy the inlined markup when you add a page.
- **The scroll-`reveal` script** sits at the bottom of `<body>`; copy it into new
  pages so `.reveal` elements fade in.

## Contact / brand facts (keep consistent across pages)
The Green House Gallery · Historic Downtown Huntsville, Utah ·
`curator@theghgallery.com` · `385-438-2783` · © 2026 The Green House Foundation,
a Utah nonprofit corporation. Keep the footer line noting Indigenous cultural
content is human-authored and human-reviewed.

---

## Voice

Plain, grounded, editorial. Short declarative sentences. "We keep art that matters
from being lost." No marketing fluff, no exclamation points, no startup-speak.
Quiet, architectural, rooted.

---

## Do NOT
- **Do not** introduce a new color, or hardcode a hex/px that already has an inline var — reference it.
- **Do not** edit the nav or footer on one page without applying the same change to the other two.
- **Do not** remove or blank the `@dsCard` comment on line 1 of a page.
- **Do not** touch `hero-loop.html`'s standalone `:root` — it's an isolated art piece.
- **Do not** add a framework, bundler, build step, or npm dependency without asking me first.
- **Do not** commit secrets, analytics keys, or real donor/contact data beyond the public info above.
