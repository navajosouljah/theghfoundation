# The Green House Foundation — website

Static marketing site for **The Green House Foundation**, a Utah 501(c)(3) that
preserves culturally significant art, develops artists into sustainable careers,
and builds cultural spaces across the Mountain West. Home of The Green House
Gallery in historic downtown Huntsville, Utah, and the Bacone collection.

This is plain hand-written HTML + CSS. **There is no build step, no framework, no
npm, no bundler.** You preview a page by opening the `.html` file in a browser.
Do not introduce a build tool, framework, or dependency without asking me first.

---

## Files

| File | What it is |
|------|-----------|
| `index.html` | The Foundation homepage. The big one (~700 lines): mission, Bacone collection, BEACON, the corridor, endowment, get involved. |
| `about.html` | About page — the valley, the three communities, why the Foundation exists. |
| `grants.html` | Grants we are actively seeking: the $5M 2026 goal, fundable priorities, grant sources. |
| `hero-loop.html` | **Standalone ambient hero animation** (CSS god-rays / breathing gradient). Self-contained art piece with its own palette. Not part of the shared page system — leave its inline `:root` alone. |
| `styles.css` | The design-system entry point. `@import` lines only — links the token files below. |
| `tokens/*.css` | The design tokens. See the token map below. |
| `assets/logos/` | Brand logo PNGs (also base64-inlined into the nav/footer of each page). |

There is **no** `CLAUDE.md` history, no tests, and no CI. If you add tooling, document it here.

---

## The design system

`styles.css` is the single entry point (`<link rel="stylesheet" href="styles.css">`)
and pulls in the token layers in this order:

```
tokens/fonts.css       → Google Fonts import (Newsreader + Inter)
tokens/colors.css      → palette + semantic aliases + .theme-cream / theme scopes
tokens/typography.css  → golden-ratio type scale, families, role tokens
tokens/spacing.css     → 8px spacing scale, radii, shadows, layout, motion
tokens/bacone.css      → the Bacone collection palette (SCOPED — see below)
tokens/base.css        → light element defaults on the .ghf scope
```

### Palette — the rules matter

The Foundation palette is **forest base, olive brand, cream light, ember accent**.

| Token | Value | Use |
|-------|-------|-----|
| `--forest` | `#20241A` | dark background base |
| `--olive` | `#858549` | **brand color** — links, brand marks, primary buttons |
| `--cream` | `#F4F5E7` | light background + text on dark |
| `--slate` | `#646463` | muted text |
| `--ember` | `#C2703D` | **accent — buttons & CTA ONLY, ~3% of any layout** |
| `--surface` | `#2C3122` | raised panels on forest |

Prefer the **semantic aliases** over raw palette in components: `--bg-page`,
`--bg-surface`, `--text-strong`, `--text-body`, `--text-muted`, `--text-brand`,
`--border-subtle`, `--accent`, `--accent-hover`, `--on-accent`, `--focus-ring`.
They re-map correctly when you switch themes.

**Themes** are scopes you add to a container:
- Default (no class): Foundation dark theme on forest.
- `.theme-cream`: light Foundation surfaces.
- `.theme-bacone`: the Bacone collection's storytelling palette (clay / sage /
  ochre / sky / ground / umber). **USE ONLY inside `.theme-bacone`. NEVER on
  Foundation pages** — it exists so shared components render correctly inside a
  Bacone storytelling block, nothing more.

### Type & spacing
- **Display / headings:** Newsreader (serif), optically variable. **Body / labels:** Inter.
- Golden-ratio scale (φ≈1.618): Display 72 / Heading 44.5 / Subhead 27.5 / Body 17 / Caption 10.5. Eyebrow 12px, uppercase, `0.18em` tracking.
- Spacing is an **8px rhythm** (`--space-1`…`--space-10`). Radii are gently architectural, never pill-soft on Foundation surfaces (`--radius-pill` is for chips/toggles only). Motion is calm — no bounce.

---

## The thing you have to know before editing a page

**The pages currently do NOT consume the token classes.** Every page links
`styles.css` but then declares its own big inline `<style>` block with a *parallel*
palette (`--ink`, `--paper`, `--clay`, `--olive`, `--ember`, …) and its own
component classes (`.wrap`, `.btn`, `.display`, `.eyebrow`, `header.nav`, `.ft`).
So there are effectively two systems living side by side.

When you touch a page, follow these rules in order:

1. **Never introduce a third palette.** Use a value that already exists — a token
   from `tokens/`, or the inline var already defined at the top of that page.
2. **Never hardcode a hex/px that already has a name.** If `--olive` exists, write
   `var(--olive)`, not `#858549`.
3. **Prefer the token layer.** When adding new CSS, reach for the semantic tokens
   (`var(--bg-surface)`, `var(--text-strong)`) rather than re-deriving colors inline.
4. **Keep `--ember` to buttons and CTAs only** (~3% of the layout). It is the one
   loud color. Do not use it for body text, borders, or backgrounds.

### Shared components are duplicated per page — edit them together
The **nav** (`header.nav`, sticky, brand + logo + `.nav-links` + a `.btn` CTA), the
**footer** (`.ft` with Explore / Visit & Contact columns + legal line), the
**buttons** (`.btn`, `.btn-olive`, `.btn-cream`, `.btn-cream-ghost`, `.btn-sm`), and
the `.wrap` container are copy-pasted into `index.html`, `about.html`, and
`grants.html`. **If you change one, change all three,** or they drift. The nav's
current link `active` state is set per page.

### Conventions to preserve
- **`@dsCard` comment** on line 1 of each page (`group`, `viewport`, `name`,
  `subtitle`) — it's design-gallery metadata. Keep it, and update `name`/`subtitle`
  when you add a page.
- Fonts are loaded **twice on purpose**: once via `styles.css`'s `@import`, and
  again as a `<link>` in each page's `<head>` (with `preconnect`) so the page renders
  fast even before the stylesheet parses. Keep the `<head>` link.
- Logos are **base64-inlined** into the nav and footer so a page renders even if
  `assets/` doesn't load. When you add a page, copy the inlined logo markup.
- The scroll-`reveal` animation script sits at the bottom of `<body>`. Copy it into
  new pages so `.reveal` elements fade in.

### Contact / brand facts (keep consistent across pages)
The Green House Gallery · Historic Downtown Huntsville, Utah ·
`curator@theghgallery.com` · `385-438-2783` · © 2026 The Green House Foundation,
a Utah nonprofit corporation. Indigenous cultural content is human-authored and
human-reviewed — keep that line in the footer.

---

## Voice

Plain, grounded, editorial. Short declarative sentences. "We keep art that matters
from being lost." No marketing fluff, no exclamation points, no startup-speak.
Quiet, architectural, rooted — the same register as the token-file comments.

---

## Do NOT
- **Do not** hardcode a color/size that already has a token or inline var — reference it.
- **Do not** use `--ember` for anything but buttons/CTAs.
- **Do not** use the Bacone palette (`clay`/`sage`/`ochre`/`sky`/`ground`/`umber`) outside `.theme-bacone`.
- **Do not** edit the nav or footer on one page without applying the same change to the other two.
- **Do not** remove or blank the `@dsCard` comment on line 1 of a page.
- **Do not** touch `hero-loop.html`'s standalone `:root` — it's an isolated art piece.
- **Do not** add a framework, bundler, build step, or npm dependency without asking me first.
- **Do not** commit secrets, analytics keys, or real donor/contact data beyond the public info above.
