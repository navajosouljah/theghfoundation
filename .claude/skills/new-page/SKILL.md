---
name: new-page
description: >
  Scaffold a new Green House Foundation website page that matches the existing
  pages exactly — same @dsCard header, <head>, inlined nav, footer, buttons,
  palette vars, and reveal script. Use when adding a page to this site (e.g.
  "add an Events page", "make a Board page", "new page for the endowment").
  Do NOT use for editing an existing page or for hero-loop.html.
---

# new-page — scaffold a Foundation page

## What this is for
Create a brand-new `.html` page at the repo root that is visually and structurally
identical to `index.html` / `about.html` / `grants.html`, so a new page never drifts
from the shared nav, footer, buttons, palette, or type. This kills the copy-paste
errors that come from hand-building a page from scratch.

## Before you start
Read `CLAUDE.md` (palette rules, the two-systems tension, shared-component rule).
Use **`about.html` as the canonical template** — it's the smallest complete page
(nav + hero + sections + footer + reveal script).

## Process
1. **Get the page's identity.** You need a filename (`<slug>.html`, lowercase,
   hyphenated), a page `name` and one-line `subtitle` for the `@dsCard`, a `<title>`
   (`<Page> | The Green House Foundation`), and a `<meta name="description">`. If the
   user didn't give these, infer sensible ones from the request — don't stop to ask
   unless the purpose is genuinely unclear.
2. **Copy `about.html` verbatim** as the starting point for the new file.
3. **Swap the identity fields:** line-1 `@dsCard` (`name`, `subtitle`), `<title>`,
   `<meta name="description">`.
4. **Set the nav active state:** if the new page maps to a nav item, add `active` to
   that `.lnk`. If it's a new top-level destination the user wants in the nav, add the
   link to the `.nav-links` block — **and add the same link to the nav on
   `index.html`, `about.html`, and `grants.html`** (shared component rule). Add it to
   the footer "Explore" column on all pages too.
5. **Replace the page body** (everything between the nav `</header>` and the
   `<!-- FOOTER -->` comment) with the new page's sections. Build sections from the
   existing class vocabulary — `.wrap`, `.eyebrow`, `.display`, section headers
   (`.shead`), `.reveal` for scroll-in, `.btn .btn-olive` / `.btn-cream` for CTAs.
   Do not invent new component classes if an existing one fits.
6. **Keep untouched:** the inlined base64 logo in nav + footer, the footer
   contact/legal block, the `reveal` `<script>` at the bottom of `<body>`, the
   `styles.css` link, and the Google Fonts `<link>` in `<head>`.
7. **Honor the palette rules:** only use vars already defined at the top of the page
   or tokens from `tokens/`. `--ember` on buttons/CTAs only. No new hex values.
8. **Verify** by opening the file in a browser (or the `run`/`verify` skill if
   present). Confirm: nav renders with correct active link, fonts load, footer
   matches the other pages, no raw hex slipped in.

## Output format
- One new file: `<slug>.html` at the repo root.
- If nav/footer links changed, edited `index.html`, `about.html`, `grants.html` too.
- A short summary to me: the filename, its `@dsCard` name/subtitle, which sections
  you built, and which other files you touched for nav/footer consistency.

## Example of a great result
> **Request:** "Add an Events page for gallery openings and workshops."
>
> **Result:** Created `events.html`.
> - Line 1: `<!-- @dsCard group="Foundation Website" viewport="1280x900" name="Events page" subtitle="Openings, workshops, and gatherings at The Green House Gallery" -->`
> - `<title>Events | The Green House Foundation</title>`, matching meta description.
> - Nav copied from `about.html` with a new `Events` link marked `active`; the same
>   `Events` link added to the nav and footer Explore column on `index.html`,
>   `about.html`, `grants.html`.
> - Body: an `.ahero` intro, a `.reveal` list of upcoming events using `.wrap`
>   sections, and a `.btn .btn-olive` "Plan your visit" CTA (the only `--ember`-free
>   olive CTA — ember reserved for the RSVP button).
> - Footer, logo, and reveal script carried over unchanged.
> - Verified in browser: nav active state correct, no raw hex, footer identical to siblings.

## Do NOT
- **Do not** build the page from an empty file or a generic HTML boilerplate — always start from `about.html`.
- **Do not** add a nav/footer link to the new page only — update all three shared pages or none.
- **Do not** introduce a new palette, new hex values, or `--ember` outside buttons/CTAs.
- **Do not** drop the `@dsCard` comment, the inlined logo, the fonts `<link>`, or the reveal script.
- **Do not** touch `hero-loop.html` — it is not part of this page system.
- **Do not** add a build step, template engine, or JS framework to "DRY up" the shared markup unless I ask for it.
