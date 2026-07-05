---
name: token-audit
description: >
  Audit the Green House Foundation site's HTML/CSS for design-system drift —
  raw hex/px values that should be tokens, ember misuse, Bacone palette leaking
  onto Foundation pages, and nav/footer that has diverged between pages. Use
  before shipping visual changes or when asked to "check the styling",
  "lint the tokens", or "find design-system violations". Reports; does not
  auto-fix unless asked.
---

# token-audit — find design-system drift

## What this is for
The pages carry a parallel inline palette alongside the `tokens/` system
(see `CLAUDE.md`), so it's easy for a raw color, a misused accent, or a drifted
nav to slip in. This skill scans for those and reports them so the site stays
one coherent system.

## Before you start
Read `CLAUDE.md` for the palette rules and the known "two systems" tension.
Know the canonical values: `--forest #20241A`, `--olive #858549`,
`--cream #F4F5E7`, `--slate #646463`, `--ember #C2703D`, `--surface #2C3122`,
and the Bacone set (`clay #C2704F`, `sage #97A782`, `ochre #DAB877`,
`sky #88A3AE`, `ground #F0E7D6`, `umber #5E4334`).

## Process
Run these checks across `index.html`, `about.html`, `grants.html`, `styles.css`,
and `tokens/*.css` (skip `hero-loop.html` — it's an intentional standalone).

1. **Raw values with a token twin.** Find hex colors and px sizes in the page
   `<style>` blocks and inline `style=""` attributes that match (or nearly match) a
   named token/var. Flag each as "use `var(--x)` instead of `#hex`". A hex used only
   to *define* a var at the top of the file is fine.
2. **Ember misuse.** Find `--ember` / `#C2703D` / `#A85C2F` used on anything that
   isn't a button or CTA (body text, borders, backgrounds, icons). Flag it.
3. **Bacone leakage.** Find any Bacone color (clay/sage/ochre/sky/ground/umber or
   their hex) used **outside** a `.theme-bacone` scope on a Foundation page. Flag it.
4. **Nav/footer drift.** Compare the `header.nav` markup and the `.ft` footer markup
   across the three pages. Flag links, columns, contact details, or the legal line
   that differ between pages (the active-nav-link difference is expected — ignore that).
5. **Contact/brand consistency.** Flag any page where the email, phone, address, or
   `© 2026` line disagrees with the others.
6. **Orphan check (light).** Note any `var(--x)` referencing a token that isn't
   defined in `tokens/` or the page's inline `:root`.

For each finding, capture: file, line, the offending snippet, which rule it breaks,
and the concrete fix.

## Output format
A markdown report, grouped by severity, then a one-line summary. Empty groups are
omitted. Do **not** edit files unless I explicitly asked for `--fix`.

```
# token-audit — <n> findings

## High (breaks a hard rule)
- grants.html:118 — `color:#C2703D` on `.stat-num` → ember used on text, not a
  button/CTA. Fix: use `var(--olive)` (brand) or `var(--text-strong)`.

## Medium (drift / raw values)
- about.html:141 vs grants.html:141 — nav Explore link "Endowment" present on
  grants.html, missing on about.html. Fix: add to about.html nav.
- index.html:512 — `#2C3122` inline → matches `--surface`. Fix: `var(--surface)`.

## Low (nits)
- index.html:88 — `#F4F5E7` used directly; `--cream` exists. Fix: `var(--cream)`.

Summary: 1 high, 2 medium, 1 low across 3 pages. Ember misuse is the one to fix first.
```

## Example of a great result
> Scanned 3 pages + 6 token files. Found ember bleeding onto a stat number in
> `grants.html`, a nav link that exists on two pages but not the third, and four raw
> hex values that each have a token. Reported them high→low with exact `file:line`
> and the `var(--x)` to swap in. No files changed (report-only run).

## Do NOT
- **Do not** flag hex values that are *defining* a CSS var at the top of a file — those are the source of truth.
- **Do not** flag `hero-loop.html` — its palette is meant to be standalone.
- **Do not** flag the per-page nav `active` class as drift — that's supposed to differ.
- **Do not** auto-edit files on a plain audit run; only fix when I ask for `--fix`, and then re-run the audit to confirm zero findings.
- **Do not** invent new tokens or propose a palette change — this skill enforces the existing system, it doesn't redesign it.
