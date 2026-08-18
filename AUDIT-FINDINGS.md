# DSW Cutting — pre-launch site audit

**Date:** 2026-08-18 · **Scope:** the 7 reachable pages (index, capabilities, work, gallery, about, contact, 404). The five unlinked case-study pages under `work/` were excluded by request.

**Method:** every page rendered at 1440px and 390px; scripted checks for links, anchors, alt text, heading order, WCAG contrast, tap targets, meta tags and computed styles; a full read of all copy; a full read of all CSS against `tokens.css`. Every finding below was verified in a browser — nothing here is inferred from source alone.

**Nothing has been changed.** This is the report you asked for before refinement.

---

## The short version

The site is in good shape structurally. No JavaScript errors, no horizontal overflow on mobile, no broken image or file links, one `<h1>` per page, and the content is genuinely strong. What it has is **drift** — the accumulated residue of several build iterations that were never reconciled. Three of those have crossed the line from "inconsistent" into "visibly broken."

The single most important fact: **`tokens.css` — a 313-line, 104-token design system — is not loaded by any page on the site.** No `<link>`, no `@import`. Every token in it resolves to nothing. That one omission is upstream of roughly half of everything below.

| | Count |
|---|---|
| Visibly broken right now | **6** |
| Customer-visible inconsistencies | 31 |
| Accessibility failures (WCAG AA) | 8 patterns |
| Design-system debt items | 40+ |
| Distinct font sizes shipped | **51** (a coherent scale needs ~7) |
| Distinct color literals shipped | **73** |
| Dead CSS | **~960 of 2,354 lines (41%)** |
| Design token utilisation | **0%** |

---

# P0 — Visibly broken. Fix before anyone sees this.

### 1. The homepage "How It Works" heading renders at 14.4px
`style.css:605–613` — a legacy `.process-list` block defines `.process-title` at `:612`, which overrides the live rule at `:551`.

"From First Contact to Final Part." renders **smaller than the step titles underneath it**. Every other section heading on the homepage is 36–40px; this one is 14.4px. It reads as a caption, and the section loses its heading entirely. See `evidence/v-process.png`.

**Fix:** delete `style.css:605–613`. One line-range. Highest impact-to-effort item on the site.

### 2. Oswald is used six times and never loaded
`style.css:630, 673, 688, 721, 949, 963` set `font-family: 'Oswald', sans-serif`. No page requests Oswald — the font links only load Inter and IBM Plex Mono.

Those six rules cover the `<h1>` on work, gallery and about, plus every heading, all 68 spec pills and the CTAs on capabilities. All of them currently render in a browser fallback with letter-spacing tuned for a condensed face it isn't using.

**Fix:** rewrite the six rules to Inter 600 with corrected tracking. (Loading Oswald instead would add a third typeface to a two-typeface system — don't.)

### 3. Five homepage tiles link to anchors that don't exist
`index.html` — the "What We Do" grid links to `capabilities.html#laser`, `#tube`, `#forming` (twice) and `#welding`. **None of those IDs exist** in `capabilities.html`.

This is the most-clicked element on the homepage. Every tile dumps the visitor at the top of the capabilities page instead of the section they asked for.

**Fix:** add the IDs to the corresponding sections in `capabilities.html`.

### 4. Two homepage tiles point at content that doesn't exist at all
`index.html:158` "Quality & Inspection" and `index.html:171` "Local Delivery". There is no quality/inspection section on the capabilities page, and local delivery is mentioned nowhere else on the site.

**Fix:** either add both sections to `capabilities.html` (recommended — the delivery fleet is a real differentiator) or remove the tiles.

### 5. Structural rules and dividers are invisible
`style.css` defines `--rule` as a *border shorthand* (`1px solid rgba(...)`) while 11 call sites use it as a *color* — `style.css:747×2, 750, 791, 792, 826, 827, 851, 855, 870, 871, 913`.

Confirmed in-browser: the five credential dividers in the trust block compute to `background: rgba(0,0,0,0)` with `border: 0px none` — 1px wide, 48px tall, and completely invisible. Same for the industries grid lines and the trust-block section rules.

**Fix:** split into `--rule-color` and `--rule-border`, correct the 11 call sites.

### 6. `work.html` renders with 26 undefined CSS variables
`capabilities.html` and `work.html` load `style-work.css`, which references 26 custom properties — `--color-ink-0`, `--font-display`, `--gutter`, `--color-ember` and 22 others. **All 26 are defined in `tokens.css`, which isn't loaded.**

Measured effect: linking `tokens.css` changes `work.html`'s page height by 219px and `capabilities.html`'s by 38px. So this is affecting layout today, not just theming. Downstream symptoms include an invisible CTA button on work.html and zero desktop side-padding.

**Fix:** add `<link rel="stylesheet" href="tokens.css">` **before** `style.css` on all 7 pages, then delete the shadow `:root` block at `style.css:3–32`. Order matters — `style.css` redefines several token names with different values (`--navy` becomes `#141c2e`, not `#0D2B4E`), so this needs a regression pass immediately after.

---

# P1 — Customer-visible inconsistency

## Facts that contradict each other

**The "one roof" contradiction.** `about.html:245` describes a second North Birmingham building with three machines. That directly contradicts `index.html:418` "Single facility, one roof," `gallery.html:380` "Twelve fiber lasers under one roof," and the four-tube-laser count stated on three other pages. A customer who reads both pages notices. Pick one and make it true everywhere.

**The laser count appears to shrink.** The homepage says "8 fiber lasers." Capabilities says "7× Mitsubishi Fiber Lasers." Gallery says "12 fiber lasers." These *do* reconcile — 7 Mitsubishi + 1 Bescutter Giga = 8 flat, plus 4 tube lasers = 12 — but nothing on the site says so, so the journey from homepage to capabilities reads as a walk-back.
**Fix:** `index.html:100` → "8 flat fiber lasers"; `capabilities.html:45` → "7× Mitsubishi Fiber Lasers + 1× Bescutter Giga."

**Robotic bending cell — singular or plural?** `about.html:244` says "cells," `capabilities.html:96` says one cell paired with a press brake.

## Terminology drift

| Thing | Called | Where |
|---|---|---|
| Forming capability | "Press Brake & Forming" / "Press Brake & Robotic Bending" / "Press Brake & Rolling" / "Press Brake & Roll" | index 123, index 120 alt, capabilities 93, gallery 459 |
| Primary CTA | "Contact Us" / "Get a Quote" / "Send Message" | everywhere / work 25 + 404 43 / contact 66 |

**The nav CTA changes label on exactly one page.** `work.html:25` says "Get a Quote" where all other pages say "Contact Us." The site's single most repeated element should not drift. (I introduced the same inconsistency on `404.html:43` — that one's on me.)

## Unexplained acronyms in customer-facing copy

`index.html:255` "FAIR" and `index.html:311` "VMI" sit in the homepage card rail with no expansion. A procurement engineer knows them; a plant manager or owner may not.
**Fix:** "First-Piece" and "Managed Inventory."

## Typography of measurements

`capabilities.html` uses a straight `"` for inches at lines 53, 56, 75, 81, 98, 100, 101, 102, 105. `index.html` uses the proper prime `″` at 69, 100, 113, 126. The same measurement is typeset two visibly different ways on the two pages a buyer compares most closely.

## One claim I'd cut

`capabilities.html:81` — **"the deepest tube capability in the Southeast."** It's the site's only unsupportable superlative, and it sits in your strongest section. Four dedicated tube lasers with 40 ft outbound is a fact that does the same work and can't be challenged. Your boss will be asked to defend the superlative; he won't be asked to defend the machine count.

## Duplicated copy

`contact.html:38–39` repeats `index.html:448–449` verbatim. On the contact page it stacks two "let's" headings and delays the form with a pitch the visitor has already accepted by arriving there.

## SEO and metadata

- **Six of seven pages have no meta description** (all except work). Google is currently writing your homepage snippet for you.
- **No Open Graph tags anywhere.** Any time this link is shared — in an email to your boss, in a LinkedIn post, in a text — it renders as a bare URL with no image, title or description.
- **No canonical tags.**
- `about.html:6` uses a different title pattern from the other five.

## Navigation

`index.html` and `contact.html` have no `class="active"` in the nav; the other four content pages do. On the contact page in particular, nothing indicates where you are.

---

# P2 — Accessibility (WCAG 2.1 AA)

All verified by computing actual contrast ratios in the browser.

| Element | Ratio | Needs | Where |
|---|---|---|---|
| "EVERY DAY ON THE FLOOR" eyebrow | **1.1** | 4.5 | work.html — effectively invisible, see `evidence/v-work.png` |
| Tile captions over photos | **1.1** | 4.5 | work.html visual strip — white on light concrete |
| Gallery address line | **1.0** | 4.5 | gallery.html |
| Timeline step numbers 01/02/03 | **2.38** | 4.5 | index.html process section |
| Credential labels ("FOUNDED", "SCALE") | **2.39** | 4.5 | index.html trust block |
| "Flat" material label | **2.86** | 4.5 | capabilities.html |
| "Contact Us" nav CTA, white on orange | **3.78** | 4.5 | capabilities.html |

**Other accessibility items:**

- **Heading order skips on `capabilities.html`** — h1 straight to h3, no h2. Screen-reader users lose a level of structure.
- **One image with empty alt** on `gallery.html`.
- **Tap targets below 44px on mobile** — the mobile nav links are 15px tall (Capabilities, Work, The Shop, About, Contact) and the social icons are 20×23px. Everything is reachable, but on a phone it's fiddly.
- **No `loading="lazy"`** on any image across the site — 61 images total, all eagerly loaded. The gallery page loads 17 at once.

---

# P3 — Design system debt

Invisible to a customer today. This is what stops the drift from coming back.

| Axis | Shipped | Coherent target |
|---|---|---|
| Color literals | 73 (27 hex + 46 rgba) | ~12 base + one alpha ramp |
| Font sizes | 51 | 7 |
| Letter-spacing values | 21 | 4 |
| Line-height values | 17 | 4 |
| Spacing values | 35 (24 off-scale, 13 off even the 4px grid) | 13 |
| Eyebrow component variants | 17 | 1 + 3 modifiers |
| Button variants | 7 (2 fills, 3 paddings, 4 sizes, 2 radii, 2 casings) | 1 + 3 modifiers |
| Pill variants | 9 | 1 + 2 modifiers |
| Inline `style=""` attributes | 52 (34 in repeated identical blocks) | ~4 |
| Dead selectors | 139 | 0 |

**Five near-identical navies** are in play — `#0D2B4E`, `#141c2e`, `#1b2540`, `#243050`, `#1e2d3d`, `#0E2954` — which is why headings on index, work and gallery are three different colors. Collapsing to two (`#0D2B4E` for surfaces, `#081B33` for footer and scrims) is the change that makes the site read as one brand instead of three iterations.

**The nav logo never shrinks on mobile.** Seven inline `style="height:36px;"` attributes outrank the media query at `style.css:1045` that would take it to 28px. Deleting seven attributes fixes it on all seven pages.

**`capabilities.html` loads 28KB of CSS it doesn't use** — `style-work.css` is 58% dead selectors, and capabilities uses almost none of it.

---

# What I'd do, in order

**Before your boss sees it** — half a day, all mechanical:

1. Delete `style.css:605–613` (the 14.4px heading)
2. Replace Oswald with Inter in six rules
3. Add the missing anchor IDs to `capabilities.html`
4. Fix the `--rule` shorthand and its 11 call sites
5. Link `tokens.css` on all 7 pages, delete the shadow `:root`, regression-check
6. Nav CTA to "Contact Us" on work and 404; add the two missing active states
7. Meta descriptions on six pages, Open Graph tags site-wide

**Before the domain moves** — content decisions that need your judgment:

8. Resolve the "one roof" contradiction (needs a factual answer from you)
9. Reconcile the laser counts with the "8 flat / 4 tube / 12 total" framing
10. Cut the "deepest in the Southeast" claim
11. Unify the forming-capability name
12. Rewrite the duplicated contact-page intro
13. Fix the seven contrast failures
14. Add the missing quality/inspection and delivery sections to capabilities

**Whenever** — the P3 debt. It costs nothing to defer, but every future edit is slower until it's done.

---

Two full sub-reports accompany this summary: `copy-report.md` (all 9 copy categories, every finding with file:line and a recommended replacement) and `design-report.md` (all 8 design-system categories with the same). Screenshots referenced above are in `evidence/`.
