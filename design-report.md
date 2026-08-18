All findings verified. Here is the report.

---

# DSW Cutting — Visual Design System Consistency Audit

**Scope:** `tokens.css`, `style.css`, `style-work.css`, and the 7 pages `index.html`, `capabilities.html`, `work.html`, `gallery.html`, `about.html`, `contact.html`, `404.html`. The `work/` subdirectory is excluded except where it establishes that a selector is used somewhere.

## THE HEADLINE FINDING (read this first)

**`tokens.css` is never loaded by any page on the site.**

`grep -rn "tokens.css"` returns exactly one hit — a code comment at `style-work.css:4`. No `<link rel="stylesheet" href="tokens.css">` and no `@import` exists anywhere. Every one of the 7 pages loads only `style.css` (plus `style-work.css` on `capabilities.html` and `work.html`).

Consequences, all confirmed:

1. **The entire design system is inert.** All 27 color tokens, 10 type-scale steps, 11 spacing steps, 3 radii, 4 shadows, 5 easings and 6 duration tokens defined in `tokens.css` resolve to nothing at runtime.
2. **`style.css:3–32` silently re-declares a *second, different* `:root` palette** that shadows the token names. `--navy` becomes `#141c2e` (not `#0D2B4E`), `--muted` becomes `#94a3b8` (not `#6B7A8D`), `--rule` becomes a *border shorthand* `1px solid rgba(255,255,255,0.10)` (not the color `#D1D9E0`), `--border` becomes a color (not a shorthand). This is why `#0D2B4E` appears hardcoded 21 times — the author could not use `var(--navy)` because it no longer means navy.
3. **26 CSS custom properties are referenced but never defined** in any loaded stylesheet — see §1.3. This produces real, visible breakage on `work.html`.

Everything in §1–§8 below flows from this. Fix this one thing and roughly half the report becomes mechanical cleanup.

---

## 1. TOKEN ADHERENCE

### 1.1 What tokens exist (`tokens.css`, 313 lines)

| Group | Tokens | Lines |
|---|---|---|
| Color v2 | `--navy #0D2B4E`, `--navy-deep #081B33`, `--steel #2B3038`, `--steel-light #E6E8EB`, `--white #FFFFFF`, `--brand #2A669F`, `--brand-light #5B9FD4`, `--ember #D97706`, `--ember-light #FBB83F`, `--off-white #F5F7FA`, `--rule #D1D9E0`, `--muted #6B7A8D`, `--error #C0392B`, `--navy-scrim-40/70/90` | 16–39 |
| Color v3 | `--color-bg-0 #0E2954`, `--color-bg-1 #1A3A6E`, `--color-bg-2 #234A85`, `--color-bg-light #F4F2EE`, `--color-bg-light-alt #E8E4DD`, `--color-ink-0/1/2/3/dark`, `--color-steel-blue #3B69A8`, `--color-steel-deep #0E2954`, `--color-ember #C9591F`, `--color-ember-hot #F26B1F`, `--color-rule #2E4F82`, `--color-rule-light #D6D2CB` | 281–304 |
| Type | `--font-display/body/mono`; `--text-xs 0.75` → `--text-6xl 5rem` (10 steps); `--weight-regular/medium/semibold/bold`; `--leading-tight/snug/normal/loose`; `--tracking-tight/normal/wide/widest` | 48–80 |
| Spacing | `--space-1` 0.5rem → `--space-20` 10rem (11 steps, 8px base); `--section-gap`, `--section-gap-sm` | 87–101 |
| Layout | `--container 1280px`, `--container-wide 1440px`, `--container-narrow 760px`, `--gutter clamp(1.25rem,4vw,3rem)` | 107–111 |
| Radii/border | `--radius-sm 3px`, `--radius-md 6px`, `--radius-lg 12px`, `--border-width 1px`, `--border-color`, `--border` | 117–123 |
| Shadow | `--shadow-sm/md/lg/xl` | 129–136 |
| Motion | `--ease-out/in/inout`, `--duration-fast/base/slow`, `--anim-duration`, `--anim-delay-step`; v3: `--ease-out-3`, `--ease-in-out-3`, `--dur-fast/base/slow/reveal` | 142–312 |
| Z-index | `--z-below/base/raised/overlay/nav/modal` | 157–162 |

**Token utilisation rate: 0%.** Not one is reachable at runtime.

### 1.2 Hardcoded values that duplicate an existing token

| File:line | Hardcoded value | Should be | Note |
|---|---|---|---|
| `style.css:71` | `background: #0D2B4E` | `var(--navy)` | `.nav-cta:hover` |
| `style.css:147` | `background: #0D2B4E` | `var(--navy)` | `.btn-primary:hover` |
| `style.css:160` | `var(--navy, #0D2B4E)` | `var(--navy)` | Fallback proves intent; resolves to `#141c2e` instead |
| `style.css:221` | `color: #0D2B4E` | `var(--navy)` | `.cap-grid-title` |
| `style.css:255` | `background: #0D2B4E` | `var(--navy)` | `.cap-tile` |
| `style.css:391` | `color: #0D2B4E` | `var(--navy)` | `.work-rail-title` |
| `style.css:429,431,431,443` | `#0D2B4E` ×4 | `var(--navy)` | `.work-rail-btn`, `.work-rail-dot.active` |
| `style.css:500` | `color: #0D2B4E` | `var(--navy)` | `.work-card-title` |
| `style.css:535` | `color: #0D2B4E` | `var(--navy)` | `.partner-text h2` |
| `style.css:542` | `background: #0D2B4E` | `var(--navy)` | `.process-section` |
| `style.css:580` | `background: #0D2B4E` | `var(--navy)` | `.timeline-node` |
| `style.css:785,816,847,906,922` | `color: #0D2B4E` ×5 | `var(--navy)` | industries/trust/credential/cta titles |
| `style-work.css:943,1086` | `#0D2B4E` ×2 | `var(--navy)` | |
| `style.css:971` | `background: #081B33` | `var(--navy-deep)` | `footer` |
| `style.css:53, 1011` | `#2B3038` ×2 | `var(--steel)` | `.nav-links a`, `.nav-hamburger span` |
| `style.css:42, 450, 1036` | `#E6E8EB` ×3 | `var(--steel-light)` | |
| `style.css:423, 439` | `#D1D9E0` ×2 | `var(--rule)` (the v2 color token) | |
| `style.css:351` | `background: #D97706` | `var(--ember)` | `.cap-spec-dot` |
| `style.css:357, 825, 869, 1042`, `style-work.css:941` | `#F5F7FA` ×5 | `var(--off-white)` | |
| `style-work.css:945` | `color: #5A6168` | `var(--color-ink-3)` | Literal copy of the token's own value |
| `style-work.css:1012` | `color: #1e2d3d` | — | |
| `style.css:712` | `var(--brand-light, #5B9FD4)` | `var(--brand-light)` | Fallback is redundant *and* differently cased from the definition at `style.css:8` |
| **rgba forms of tokens** | | | |
| `style.css:89–92, 297, 302` | `rgba(8,27,51,α)` ×8 | `--navy-scrim-*` / `color-mix(--navy-deep)` | `#081B33` decomposed |
| `style.css:43, 456, 485, 1038`, `404.html:49,53,57,61` | `rgba(13,43,78,α)` ×9 | `--navy` at alpha | `#0D2B4E` decomposed |
| `style.css:527, 528, 685, 686` | `rgba(42,102,159,α)` ×4 | `--brand` at alpha | `#2A669F` decomposed |
| `style-work.css:15, 96, 946, 991, 1056`, `gallery.html:92` | `rgba(14,41,84,α)` ×6 | `--color-bg-0` at alpha | `#0E2954` decomposed |
| `style-work.css:623, 643` | `rgba(201,89,31,α)` ×2 | `--color-ember` at alpha | `#C9591F` decomposed |
| `style-work.css:483` | `rgba(242,107,31,0.1)` | `--color-ember-hot` at alpha | `#F26B1F` decomposed |

**Fix:** load `tokens.css` before `style.css` on all 7 pages, delete the shadow `:root` at `style.css:3–32`, then convert the above. Where alpha is needed, use `color-mix(in srgb, var(--navy) 12%, transparent)` rather than a re-decomposed rgba triple.

### 1.3 Undefined variables (26) — referenced but never defined in loaded CSS

`--color-bg-0` (9×), `--color-bg-1` (8×), `--color-bg-2` (8×), `--color-bg-light` (3×), `--color-bg-light-alt` (2×), `--color-ember` (31×), `--color-ember-hot` (3×), `--color-ink-0` (20×), `--color-ink-1` (10×), `--color-ink-2` (15×), `--color-ink-3` (6×), `--color-ink-dark` (12×), `--color-rule` (27×), `--color-rule-light` (9×), `--color-steel-blue` (2×), `--dur-base` (4×), `--dur-fast` (1×), `--dur-slow` (2×), `--duration-base` (1×), `--ease-out` (3×), `--ease-out-3` (7×), `--ember` (1×), `--font-body` (1×), `--font-display` (12×), `--font-mono` (31×), `--gutter` (12×).

Only the subset reached by `work.html` renders. Confirmed live breakage on that page:

- **`style-work.css:462–474` `.case-cta__btn`** — `background: var(--color-ember)` and `color: var(--color-ink-0)` both drop. The primary CTA at `work.html:174` renders as **unstyled inherited text on the page background — no orange fill, no button**. `font-family: var(--font-mono)` also drops, so it is not even mono.
- **`style-work.css:434` `.case-cta`** — `background: var(--color-bg-1)` drops → transparent. The CTA band shows the dark `body` navy directly beneath the white `.work-visual-strip` above it, producing an abrupt unstyled white→navy seam.
- **`style-work.css:951, 1073, 445` — `padding: 0 var(--gutter)`** drops to `0`. On desktop, `.work-industry-v2__inner`, `.work-visual-strip__inner` and `.case-cta__inner` have **zero horizontal padding**; content runs flush to the viewport edge on any screen between 900px and ~1400px. Only the `@media (max-width:900px)` block (`style-work.css:1130–1131, 1142–1143`) restores padding, so mobile looks correct and desktop does not.
- **`style-work.css:934, 1068` `border-bottom: 1px solid var(--color-rule)`** drops — the dividers between industry sections and below the visual strip do not render.
- **`style-work.css:1037` `.work-industry-v2__cta`** — `font-family: var(--font-mono)` drops; the CTA renders in Inter while its `letter-spacing: 0.12em` and `font-size: 0.85rem` were tuned for IBM Plex Mono.
- **`style-work.css:994, 1119` `.work-industry-v2__badge` / `.work-visual-strip__caption`** — mono family drops, same mis-tuned tracking.
- **`style.css:1204–1209` `a.work-card:hover .work-card-title`** — `transition: color var(--duration-base) var(--ease-out)` and `color: var(--ember)` both drop. **The featured-work card hover on `index.html` does nothing at all.**

### 1.4 `--rule` is a border shorthand used as a color — 11 invalid declarations

`style.css:24` defines `--rule: 1px solid rgba(255,255,255,0.10);`. It is used correctly as a shorthand at `style.css:608, 626, 934`, but **incorrectly** — producing `1px solid 1px solid rgba(...)` or `background: 1px solid rgba(...)`, both invalid and dropped — at:

| Line | Declaration | Live effect |
|---|---|---|
| `style.css:747` ×2 | `border-top/bottom: 1px solid var(--rule)` | (dead selector) |
| `style.css:750` | `background: var(--rule)` | (dead selector) |
| **`style.css:791`** | `background: var(--rule)` on `.industries-grid` | **`index.html:193` — the 7-tile industries grid uses `gap:1px` + a background to draw its rules. Background drops to transparent, tiles are `--white` on a `--white` section → the grid has no visible lines at all.** |
| **`style.css:792`** | `border: 1px solid var(--rule)` | Same grid loses its outer frame |
| **`style.css:826, 827`** | `border-top/bottom` on `.trust-block` | **`index.html:382` — the trust section loses both separator rules** |
| `style.css:851, 855` | `.trust-divider`, `.trust-block .promise-grid` | (dead selectors) |
| `style.css:870, 871` | `.credentials-bar` | (dead selector) |
| **`style.css:913`** | `background: var(--rule)` on `.credential-divider` | **`index.html:396,401,406,411,416` — all 5 vertical dividers between credential items are invisible** |

**Fix:** rename to `--rule-color` (the `#D1D9E0` value from `tokens.css`) and `--rule-border` for the shorthand, then correct the 11 call sites. Never give a variable a name that could be read as either.

---

## 2. COLOR PALETTE

**73 distinct color literals** ship across `style.css`, `style-work.css` and inline HTML (27 hex + 46 rgba), against a token palette of 27. A coherent system for a site this size needs about 12 base colors plus a documented alpha ramp.

### 2.1 Intended palette (in tokens, some reachable via the shadow `:root`)

`#0D2B4E`, `#081B33`, `#2B3038`, `#E6E8EB`, `#FFFFFF`, `#2A669F`, `#5B9FD4`, `#D97706`, `#F5F7FA`, `#D1D9E0`, `#6B7A8D`.

### 2.2 Near-duplicate clusters — these are the ones that read as sloppiness

**Cluster A — five near-identical dark navies, plus three rgba re-spellings of them:**

| Value | Where | Source |
|---|---|---|
| `#0D2B4E` | 21 raw uses across `style.css` + `style-work.css:943,1086` | `tokens.css --navy` |
| `#0E2954` | `tokens.css:281,298`; `rgba(14,41,84,…)` at `style-work.css:15,96,946,991,1056`, `gallery.html:92` | `--color-bg-0` |
| `#141c2e` | `style.css:5,19` (`--dark`/`--navy`) | invented |
| `#1b2540` | `style.css:6,20` (`--dark-2`/`--navy-2`) | invented |
| `#1e2d3d` | `style.css:13,26`, `style-work.css:1012`, `contact.html:47,51,55,59,64` | invented |
| `#081B33` | `style.css:971`; `rgba(8,27,51,…)` ×8 | `--navy-deep` |
| `#243050` | `style.css:21` — **used exactly once** (`about.html` leadership placeholder) | invented |

Six of these sit within 15 ΔE of one another. The user-visible result: the page-hero band is `#141c2e`, the process section is `#0D2B4E`, the footer is `#081B33`, `about.html`'s origin panel is `#1b2540`, `gallery.html`'s ops section is `#141c2e`. **Five different "dark navy" section backgrounds across a 7-page site.**

**Fix:** collapse to `--navy #0D2B4E` (surfaces) + `--navy-deep #081B33` (footer/scrims). Delete `#141c2e`, `#1b2540`, `#243050`, `#0E2954`, `#1e2d3d`.

**Cluster B — six near-identical mid-greys:**

| Value | Where |
|---|---|
| `#4e6070` | `style.css:14,27,510`; `style-work.css:1018,1027,1052` |
| `#4B5563` | `style-work.css:944,946` (both overridden — see §6.4) |
| `#4a5260` | `style-work.css:526` — **used once** |
| `#5A6168` | `style-work.css:945` — **used once**, and overridden |
| `#6B7A8D` | `tokens.css:33` `--muted` — unreachable |
| `#94a3b8` | `style.css:12` — `--muted` redefined to a *different* value under the *same name* |

**Fix:** one `--muted` for light backgrounds, one `--muted-on-dark`. Delete the other four.

**Cluster C — four oranges, none of which agree:**

| Value | Where |
|---|---|
| `#D97706` | `tokens.css --ember`; hardcoded at `style.css:351` |
| `#C9591F` | `tokens.css --color-ember`; `rgba(201,89,31,…)` at `style-work.css:623,643` |
| `#F26B1F` | `tokens.css --color-ember-hot`; `rgba(242,107,31,0.1)` at `style-work.css:483` |
| **`#dd5932`** | `style.css:719` (`.cap-cta`), `style-work.css:1034` (`.work-industry-v2__cta`) — **matches no token** |
| **`#c44d2a`** | `style.css:726`, `style-work.css:1047` (hover states) — **matches no token** |

`#dd5932` is the site's actual visible accent (it is on the capabilities CTAs and the work-page industry CTAs), and it exists in no token file. `#D97706` — the *declared* accent — appears exactly once, on a 4px dot at `style.css:351`.

**Fix:** decide whether the accent is amber (`#D97706`) or burnt orange (`#dd5932`), name it `--ember`, delete the other three.

**Cluster D — near-duplicate alpha values on the same base color:**

- `rgba(42,102,159, 0.07 / 0.20 / 0.28)` across three pill components (`style.css:527,528,685,686`) plus `0.08 / 0.22` in `tokens.css:240–241` for the same pill idea. Five alphas for two visual roles.
- `rgba(255,255,255, …)` — **17 distinct alphas**: `.02 .06 .08 .10 .12 .15 .35 .38 .40 .45 .60 .68 .72 .75 .85 .88 .97`. `0.38` (`style.css:149`) vs `0.40` (`style.css:974`) vs `0.45` (`style.css:185`) are not visually distinguishable. **Fix:** define a 5-step ramp (`.08 .16 .40 .64 .88`) and snap all 17 to it.
- `rgba(8,27,51, …)` — 8 distinct alphas for hero/tile scrims.

### 2.3 Strays used once or twice — delete or tokenise

| Value | Uses | File:line | Verdict |
|---|---|---|---|
| `#cbd5e1` | 6 | `contact.html:47,51,55,59,64`; `about.html:75` | Tailwind slate-300. Near-miss of `--rule #D1D9E0`. Form-input borders and the heritage-banner paragraph. **Use `--rule`.** |
| `#64748b` | 1 | `style.css:1026` (`#lb-close`) | Tailwind slate-500. **Use `--muted`.** |
| `#bae6fd` | 1 | `style.css:965` | Tailwind sky-200 — a *sky-blue* text color in a brand-blue system. Dead (`.mat-tag` unused). **Delete.** |
| `rgba(14,165,233,0.15)` | 1 | `style.css:960` | Tailwind sky-500. Dead. **Delete.** |
| `rgba(56,189,248,0.4)` | 1 | `style.css:961` | Tailwind sky-400. Dead. **Delete.** |
| `#eaf0f7` | 1 | `style.css:31` (`--lighter-bg`) | Used only by `gallery.html`'s embedded styles. Near-miss of `--off-white #F5F7FA`. **Merge.** |
| `#f1f5f9` | 1 | `style.css:11` (`--text`) | Tailwind slate-100. Tokens' equivalent is `--color-ink-1 #E6E6E6`. **Pick one.** |
| `#243050` | 1 | `style.css:21` | **Delete.** |
| `#4a5260` | 1 | `style-work.css:526` | **Delete.** |
| `rgba(0,0,0,0.85)` / `(0.97)` / `(0.18)` / `(0.09)` | 1 each | `style-work.css:1117`, `style.css:1021, 663, 16` | Pure-black alphas in a navy-scrim system; `tokens.css:37–39` provides `--navy-scrim-*`. **Convert to navy scrims.** |

Six distinct Tailwind default-palette colors (`#cbd5e1`, `#64748b`, `#94a3b8`, `#f1f5f9`, `#4B5563`, `#bae6fd`, plus two sky rgbas) are mixed into a hand-built brand palette. That is a reliable tell of copy-paste drift.

---

## 3. TYPE SCALE

### 3.1 Font sizes — 51 distinct values against a 10-step token scale

**35 distinct fixed sizes + 16 distinct `clamp()` expressions = 51.** A coherent scale for this site needs 8–10.

The 10 token steps (`--text-xs` 0.75 → `--text-6xl` 5rem) are used **zero** times. Of the 35 fixed sizes, only **four** coincide with a token value: `0.75rem` (=`--text-xs`), `1.0625rem` (=`--text-base`), `1.25rem` (=`--text-lg`), `1.5rem` (=`--text-xl`). The other 31 are ad hoc.

**Near-identical clusters (each pair/triple is visually indistinguishable):**

| Cluster | Values | Count | Representative lines |
|---|---|---|---|
| ~10px micro | `0.6rem` (×10), `0.62rem` (×1), `0.65rem` (×8), `0.68rem` (×1) | **20 uses / 4 sizes** | `style.css:184, 898, 480, 330`; `contact.html:46,50,54,58,63` |
| ~11px label | `0.7rem` (×16), `0.72rem` (×8), `0.75rem` (×20) | **44 uses / 3 sizes** | `style.css:588, 103, 689`; `style-work.css:109, 840, 38` |
| ~13px caption | `0.78rem` (×1), `0.8rem` (×3), `0.82rem` (×5), `0.85rem` (×7) | **16 uses / 4 sizes** | `style.css:964, 909, 613, 679` |
| ~14–15px body-sm | `0.875rem` (×16), `0.9rem` (×9), `0.93rem` (×1), `0.9375rem` (×5), `0.95rem` (×4) | **35 uses / 5 sizes** | `style.css:66, 508`; `about.html:184, 38`; `style.css:141` |
| ~17px body | `1rem` (×6), `1.05rem` (×1), `1.0625rem` (×10) | **17 uses / 3 sizes** | `style.css:620, 56, 132` |
| ~22px | `1.35rem`, `1.375rem` (×3), `1.4rem` (×2) | **6 uses / 3 sizes** | `style.css:1123, 177, 1026` |
| ~28px | `1.75rem` (×3), `1.85rem` (×1) | **4 uses / 2 sizes** | `style.css:950, 674` |

`0.93rem` (`about.html:184`) vs `0.9375rem` (`style.css:141`) differ by **0.06px**. `0.875rem` vs `0.9rem` differ by 0.4px. These are not design decisions.

**Sizes appearing exactly once — each a candidate for deletion:**
`0.48rem` (`style.css:1173`), `0.55rem` (`style.css:1069`), `0.55em` (`gallery.html:119`), `0.62rem` (`style.css:898`), `0.68rem` (`style.css:330`), `0.78rem` (`style.css:964`), `0.85em` (`style-work.css:481`), `0.93rem` (`about.html:184`), `1.05rem` (`style.css:56`), `1.35rem` (`style.css:1123`), `1.65rem` (`style.css:1170`), `1.85rem` (`style.css:674`), `2rem` (`about.html:174`), `2.75rem` (`about.html:97`).

**The 16 `clamp()` expressions** — six different H2 ramps that all land in the same place:
`clamp(1.75rem,3vw,2.25rem)` (×4), `clamp(1.75rem,3vw,2.5rem)` (×7), `clamp(1.75rem,2.5vw,2.25rem)`, `clamp(1.75rem,4vw,2.5rem)`, `clamp(1.75rem,3.5vw,2.5rem)`, `clamp(1.5rem,3vw,2rem)`. Plus four H1 ramps: `clamp(2rem,4vw,3rem)` (×2), `clamp(2rem,3.5vw,2.75rem)`, `clamp(2rem,4vw,2.75rem)`, `clamp(2.25rem,4.5vw,3.5rem)`, `clamp(2.5rem,6vw,4.5rem)` (×2), `clamp(2.75rem,5.5vw,5rem)`, `clamp(1.75rem,4vw,3rem)`.

**Fix:** define `--h1`, `--h2`, `--h3` as three named clamp tokens and `--text-xs/sm/base/lg` as four fixed steps. That is 7 tokens covering all 51 current values.

### 3.2 Font weight — clean

`400` (1×), `500` (36×), `600` (49×), `700` (11×). This maps cleanly to the four weight tokens and is the one genuinely coherent axis in the system. The only note: 11 uses of `700` are concentrated in the Oswald-based rules (`style.css:630, 673, 688, 950`) and `style-work.css` case-study headings — after the Oswald fix (§3.4) the site is effectively 500/600 only.

### 3.3 Letter-spacing — 21 distinct values; ~6 needed

| Role | Values found | Should be |
|---|---|---|
| Display negative | `-0.01em` (9×), `-0.015em` (3×), `-0.02em` (9×), `-0.025em` (21×), `-0.03em` (2×) | `--tracking-tight -0.025em` + `--tracking-snug -0.01em` |
| Neutral/near-zero | `0.01em` (2×), `0.02em` (1×), `0.03em` (1×), `0.04em` (4×), `0.05em` (5×), `0.06em` (3×), `0.07em` (2×) | `--tracking-normal 0` |
| Mono / eyebrow | `0.08em` (13×), `0.09em` (1×), `0.1em` (15×), `0.12em` (14×), `0.14em` (1×) | `--tracking-wide 0.1em` |
| All-caps micro | `0.18em` (1×), `0.2em` (8×), `0.25em` (2×), `0.3em` (2×) | `--tracking-widest 0.2em` |

Tokens define exactly four (`-0.03em`, `0em`, `0.06em`, `0.12em`) — and **`0.06em` is used only 3×** while `0.1em` (not a token) is used 15× and `0.08em` 13×. The token values were chosen and then ignored.

Singletons to delete: `0.02em`, `0.03em`, `0.09em` (`style.css:481`), `0.14em` (`style-work.css:1025`), `0.18em` (`about.html:103`).

### 3.4 Line-height — 17 distinct values; 4 tokens defined, 0 used

`1` (6×), `1.02` (2×), `1.05` (2×), `1.08` (1×), `1.1` (5×), `1.15` (10×), `1.2` (6×), `1.3` (1×), `1.4` (1×), `1.45` (1×), `1.5` (3×), `1.55` (5×), `1.6` (6×), `1.65` (3×), `1.7` (7×), `1.75` (1×), `1.8` (6×).

Tokens: `--leading-tight 1.15`, `--leading-snug 1.35`, `--leading-normal 1.6`, `--leading-loose 1.8`. Note `1.35` is **never used** while `1.3` and `1.4` each appear once.

The `1.5 / 1.55 / 1.6 / 1.65 / 1.7 / 1.75 / 1.8` band is 7 values covering 31 uses of body text — visually one value. **Fix:** collapse to `1.15` (headings), `1.35` (subheads), `1.6` (body), `1.8` (captions), exactly as the tokens already prescribe.

Singletons: `1.08` (`style.css:124`), `1.3` (`style.css:503`), `1.4` (`style.css:909`), `1.45` (`style-work.css:316`), `1.75` (`about.html:74`).

### 3.5 Font families — Inter vs IBM Plex Mono, plus a ghost

**Declaration counts:** `var(--font-mono)` 31, `'IBM Plex Mono', monospace` 23, `'Inter', sans-serif` 29, `var(--font-display)` 12, `'Inter',sans-serif` (no space) 3, `var(--font-body)` 1, `inherit` 6, **`'Oswald', sans-serif` 6**.

Three problems:

1. **Oswald is never loaded.** The Google Fonts link on every page (`index.html:8` and equivalents) requests only `Inter` and `IBM Plex Mono`. Six rules — `style.css:630` (`.page-hero h1`), `:673` (`.cap-heading`), `:688` (`.cap-spec-pill`), `:721` (`.cap-cta`), `:949` (`.mat-heading`), `:963` (`.mat-tag`) — request Oswald and fall through to generic `sans-serif`. Because Oswald is a condensed face, its companion `letter-spacing: 0.04em` / `0.08em` / `0.12em` and `text-transform: uppercase` are tuned for a narrow face and look loose and wide in the fallback. **This affects the H1 on `work.html`, `gallery.html`, `about.html` and every heading, spec pill and CTA on `capabilities.html`** (68 `.cap-spec-pill` instances alone). **Fix:** delete Oswald; use Inter 600 with `--tracking-widest`.

2. **The mono/sans split is systematic in intent but inconsistently declared.** The rule is legible: IBM Plex Mono for eyebrows, spec labels, stat labels, pills and captions; Inter for everything else. But it is spelled four different ways — `var(--font-mono)` (31×, broken), the literal `'IBM Plex Mono', monospace` (23×, works), `var(--font-display)` (12×, broken), `'Inter', sans-serif` (29×). On `work.html` and `capabilities.html` the `var()` forms drop and elements silently fall back to Inter (§1.3). **Fix:** use `--font-mono` / `--font-display` everywhere once tokens load.

3. **Mono weight 600 is not loaded.** The page-level font link requests `IBM+Plex+Mono:wght@400;500`, but `style-work.css:1039` (`.work-industry-v2__cta`), `:571`, `:597`, `:654` and `style.css:689` set `font-weight: 600` on mono text → browser-synthesised faux bold. (`tokens.css:10` requests 300;400;500;600 — but is not loaded.) **Fix:** add `600` to the page-level link or drop to 500.

Also cosmetic: `'Inter',sans-serif` without the space appears at `capabilities.html:34`, `contact.html:33`, `404.html:34` while the CSS everywhere else uses `'Inter', sans-serif`.

---

## 4. SPACING RHYTHM

**35 distinct `rem` values** appear in `padding` / `margin` / `gap`.

**On the 8px token scale (11 values, 251 uses):** `0.5` (20×), `1` (45×), `1.5` (39×), `2` (39×), `2.5` (25×), `3` (43×), `4` (10×), `5` (23×), `6` (4×), `8` (2×), `10` (1×). The backbone is genuinely there.

**Off the 8px scale but on a 4px grid (10 values, 107 uses)** — defensible as half-steps, but undeclared: `0.25` (8×), `0.75` (28×), `1.25` (48×), `1.75` (13×), `2.75` (1×), `3.5` (10×), `4.5` (2×), `5.5` (1×), `7` (2×), `9` (1×), `11` (1×).

**Off even the 4px grid — 13 values, 56 uses. These are the rhythm breakers:**

| Value | px | Uses | Example lines | Snap to |
|---|---|---|---|---|
| `0.2rem` | 3.2 | 1 | `style.css:174` (`.stat-content` gap) | `0.25rem` |
| `0.3rem` | 4.8 | 2 | `style.css:900, 906` (credential label/value margins) | `0.25rem` |
| `0.35rem` | 5.6 | 5 | `style.css:1060, 1112`; `style-work.css:730`; `about.html:143` | `0.25rem` |
| `0.4rem` | 6.4 | **18** | `style.css:435, 517, 683, 927`; `contact.html:46,50,54,58,63`; `about.html:100,139` | `0.5rem` |
| `0.45rem` | 7.2 | 1 | `style.css:958` | `0.5rem` |
| `0.55rem` | 8.8 | 4 | `style-work.css:554, 566, 223` | `0.5rem` |
| `0.6rem` | 9.6 | 6 | `style.css:111, 212, 382`; `gallery.html:260` | `0.5rem` |
| `0.65rem` | 10.4 | 1 | `style.css:1044` | `0.75rem` |
| `0.8rem` | 12.8 | 6 | `style-work.css:377, 730, 783`; `style.css:1112` | `0.75rem` |
| `0.85rem` | 13.6 | 5 | `style.css:1042, 1055, 1060`; `style-work.css:588` | `0.75rem` |
| `0.9rem` | 14.4 | 4 | `style.css:760`; `style-work.css:123, 554, 998` | `1rem` |
| `1.1rem` | 17.6 | 3 | `style.css:39` (**nav vertical padding**), `:511`, `:675` | `1rem` |
| `1.15rem` | 18.4 | 1 | `style.css:1044` | `1.25rem` |

`style.css:39` is worth calling out on its own: the fixed nav uses `padding: 1.1rem 3rem`, giving a nav height that is not a multiple of 4 or 8. Since `.hero` compensates with `padding-top: 72px` (`style.css:77`) and `style-work.css:94` uses `top: 64px` while `style-work.css:755` uses `top: 68px`, **three different assumptions about the nav's height coexist** — 72px, 64px and 68px. Only one can be right.

Additionally: `0.4rem` at 18 uses is the single most common off-grid value, and 9 of those 18 are in `contact.html`'s inline form styles (§5).

**Fix:** the token scale already exists (`--space-1` … `--space-20`). Add `--space-0-5: 0.25rem` and `--space-1-5: 0.75rem` as the only sanctioned half-steps, then snap the 13 off-grid values above. That takes 35 distinct values → 13.

---

## 5. INLINE STYLES

**52 `style=""` attributes across the 7 pages.** By file: `contact.html` 18, `capabilities.html` 13, `404.html` 8, `about.html` 7, `index.html` 2, `work.html` 2, `gallery.html` 2.

### 5.1 Repeated identical blocks (34 of the 52)

| Block | Count | File:line | Fix |
|---|---|---|---|
| `height:36px;` on nav logo | **7** (all pages) | `index.html:15`, `capabilities.html:16`, `work.html:17`, `gallery.html:347`, `about.html:211`, `contact.html:15`, `404.html:16` | **Delete entirely** — see §5.3, this is a live bug |
| `height:28px;opacity:0.7;` on footer logo | **7** (all pages) | `index.html:459`, `capabilities.html:281`, `work.html:182`, `gallery.html:541`, `about.html:329`, `contact.html:93`, `404.html:70` | Add `.footer-logo { height: 28px; opacity: 0.7; }` |
| `font-family:'Inter',sans-serif;font-weight:600;letter-spacing:-0.025em;font-optical-sizing:auto;` on `<h1>` | **3** | `capabilities.html:34`, `contact.html:33`, `404.html:34` | Fix `.page-hero h1` (`style.css:629`) instead — see §6.1 |
| `display:block;font-size:0.6rem;letter-spacing:0.2em;text-transform:uppercase;color:var(--brand);margin-bottom:0.4rem;` on `<label>` | **5** | `contact.html:46, 50, 54, 58, 63` | `.form-label` |
| `width:100%;background:#ffffff;border:1px solid #cbd5e1;padding:0.75rem 1rem;color:#1e2d3d;font-size:0.875rem;font-family:inherit;box-sizing:border-box;` on `<input>` | **4** | `contact.html:47, 51, 55, 59` | `.form-input` |
| Same + `resize:vertical` on `<textarea>` | 1 | `contact.html:64` | `.form-input` + `textarea.form-input` |
| `color:inherit;text-decoration:none;border-bottom:1px solid rgba(13,43,78,0.25);` | **4** | `404.html:49, 53, 57, 61` | `.contact-val a` |
| `object-position: center;` on `.cap-bg img` | **8** | `capabilities.html:39, 64, 89, 113, 134, 156, 176, 240` | Redundant — `object-position` already defaults to `center`. **Delete all 8.** |

The `contact.html` form is the worst case: **10 inline blocks, 3 unique**, carrying 5 hardcoded literals (`#ffffff`, `#cbd5e1`, `#1e2d3d`, `0.875rem`, `0.75rem 1rem`) each repeated 4–5 times. Any change to form styling currently requires 10 coordinated edits.

### 5.2 One-off inline styles

| File:line | Style | Assessment |
|---|---|---|
| `capabilities.html:197` | `object-position: center 30%;` | Legitimate per-image art direction. Keep, or move to a `data-` driven utility. |
| `capabilities.html:219` | `object-position: center 15%;` | Same. |
| `about.html:228` | `max-width:1100px; margin:0 auto;` | **Layout drift** — no other page constrains `.page-hero` content. See §6.1. |
| `about.html:240` | `color:var(--brand-light);border-color:var(--brand-light);` on `.section-eyebrow` | Should be a `.section-eyebrow--light` modifier |
| `about.html:253` | `font-size:0.6rem;letter-spacing:0.3em;text-transform:uppercase;color:var(--brand-light);margin-bottom:0.75rem;` | A hand-rolled duplicate of `.section-eyebrow` minus the border. Make it `.section-eyebrow--plain`. |
| `about.html:285` | `color:var(--brand);border-color:var(--brand);` on `.section-eyebrow` | Should be `.section-eyebrow--brand` |
| `about.html:320` | `width:100%;justify-content:center;display:flex;` on `.cta-buttons` | **Redundant** — `.cta-buttons` (`style.css:924`) already sets `display:flex; justify-content:center`. Delete. |
| `contact.html:41` | `max-width:680px;margin:2.5rem auto 0;text-align:left;` on `<form>` | `.quote-form` |
| `contact.html:43` | `display:none;` on honeypot | Acceptable, or `.visually-hidden` |
| `contact.html:44` | `display:grid;grid-template-columns:1fr 1fr;gap:1rem;margin-bottom:1rem;` | `.form-row` — **and this is what `style.css:1156` targets via `.cta-band form > div:first-of-type`**, a fragile structural selector that exists only because the class is missing |
| `contact.html:62` | `margin-bottom:1.25rem;` | `.form-field` |
| `contact.html:66` | `width:100%;cursor:pointer;border:none;font-family:inherit;font-size:0.875rem;` on `.btn-primary` | **Overrides `.btn-primary`'s `font-size: 0.9375rem`** (`style.css:141`) → the site's one form submit button is 1px smaller than every other primary button. Add `.btn-primary--block` and a base `button.btn-primary` reset. |
| `404.html:42` | `margin-bottom:2.5rem;` on `.cta-buttons` | `.cta-buttons--spaced` |

### 5.3 The nav-logo inline height is a live mobile bug

`style.css:49` sets `.nav-logo img { height: 36px; }` and `style.css:1045` (inside `@media (max-width:768px)`) sets `height: 28px`. **The inline `style="height:36px;"` on all 7 pages beats both**, because inline styles outrank any author rule regardless of media query. The mobile logo shrink never fires on any page. Deleting the 7 inline attributes fixes it — the CSS is already correct.

---

## 6. COMPONENT CONSISTENCY

### 6.1 `.page-hero` — drifts on every axis (6 pages)

| Page | Line | Content wrapper | `<h1>` treatment | Rendered family / weight / tracking |
|---|---|---|---|---|
| `capabilities.html` | 32 | none | inline override | Inter / 600 / −0.025em, **uppercase** |
| `contact.html` | 31 | none | inline override | Inter / 600 / −0.025em, **uppercase** |
| `404.html` | 32 | none | inline override | Inter / 600 / −0.025em, **uppercase** |
| `work.html` | 33 | none | **inherits `.page-hero h1`** | **fallback sans-serif / 700 / +0.03em**, uppercase |
| `gallery.html` | 363 | none | **inherits** | **fallback sans-serif / 700 / +0.03em**, uppercase |
| `about.html` | 227 | **`<div style="max-width:1100px;margin:0 auto;">`** | **inherits** | **fallback sans-serif / 700 / +0.03em**, uppercase |

Three pages get Inter 600 with tight negative tracking; three get a fallback grotesque at 700 with **positive** tracking — a 0.055em swing plus a 100-weight swing. Side by side these do not read as the same site. The inline overrides at `capabilities.html:34`, `contact.html:33` and `404.html:34` are a partial, undocumented fix that was never applied to the other three.

`about.html` additionally centres its hero content at 1100px while the other five run flush-left at the section's `3rem` padding — so the eyebrow and H1 start at a different x-position on that one page.

The `.page-hero` band itself is `background: var(--navy)` = `#141c2e` — a navy used nowhere else on those pages' content sections.

**Fix:** rewrite `style.css:629–636` to Inter / 600 / `--tracking-tight`, drop `text-transform: uppercase` (or keep it consistently), delete the 3 inline overrides and the `about.html:228` wrapper, and add `.page-hero__inner { max-width: var(--container); margin-inline: auto; }` applied on all 6.

### 6.2 CTA band — two entirely different components

| Page | Line | Component | Background | Button |
|---|---|---|---|---|
| `index.html` | 447 | `.cta-band` | `--off-white` `#f5f7fa` | `.btn-primary` (brand blue, 4px radius) |
| `gallery.html` | 529 | `.cta-band` | same | same |
| `about.html` | 317 | `.cta-band` + inline override | same | same |
| `contact.html` | 37 | `.cta-band` + embedded form | same | `.btn-primary` + inline `font-size:0.875rem` |
| `404.html` | 38 | `.cta-band` + `.contact-row` | same | same |
| **`work.html`** | **171** | **`.case-cta`** (from `style-work.css`) | **`var(--color-bg-1)` → undefined → transparent** | **`.case-cta__btn` — orange fill and text color both drop; renders as bare text** |

`work.html` uses a different CTA component from every other page, and that component is broken. Six pages should share one CTA band. **Fix:** replace `work.html:171–177` with the standard `.cta-band` / `.btn-primary` markup.

### 6.3 Nav and footer — structurally identical (the good news)

The `<nav>` block is byte-identical across all 7 pages apart from (a) which link carries `class="active"` and (b) `404.html` using absolute `/` paths. The `<footer>` is byte-identical across all 7 apart from the same `/` paths on `404.html`. Both are correct.

Two minor drifts:
- **`work.html:24`** — the nav CTA reads `Get a Quote`; the other 6 pages read `Contact Us`. Same for `404.html:43` (`Get a Quote`) vs the `.btn-primary` label on the other CTA bands (`Contact Us`). As a *component*, the primary CTA has two labels; pick one so the button width is stable across pages.
- `index.html` has no `.active` nav state (Home is not in `.nav-links`), and `contact.html` marks nothing active even though it is the current page. Minor, but the active-state contract is incomplete.

### 6.4 `.work-industry-v2` — self-contradicting declarations in one file

`style-work.css` declares the same selectors twice at equal specificity, with different colors. The later block wins, silently discarding the earlier:

| Selector | Line 937–947 declares | Line 1001–1057 declares | Winner |
|---|---|---|---|
| `.work-industry-v2__title` | `color: #0D2B4E` (943) | `color: #1e2d3d` (1012) | **`#1e2d3d`** |
| `.work-industry-v2__context` | `color: #4B5563` (944) | `color: #4e6070` (1018) | **`#4e6070`** |
| `.work-industry-v2__parts-heading` | `color: #5A6168` (945) | `color: #4e6070` (1027) | **`#4e6070`** |
| `.work-industry-v2__note` | `color: #4B5563` (946) | `color: #4e6070` (1052) | **`#4e6070`** |

Net effect: `work.html`'s industry headings render at `#1e2d3d`, not the `#0D2B4E` the file explicitly asks for at line 943 — so **`work.html` headings are a different navy from `index.html`'s** (`#0D2B4E` at `style.css:785, 816, 847`) and from `gallery.html`'s (`--dark` `#141c2e`). Three heading colors across three pages, all intended to be "the dark navy."

`#4B5563` and `#5A6168` are dead code that also happen to be two of the six greys in Cluster B (§2.2).

**Fix:** delete `style-work.css:943–946` outright; set the surviving rules to `var(--navy)` / `var(--muted)`.

### 6.5 Eyebrows — 17 variants of one component

| Class | File:line | Family | Size | Tracking | Color |
|---|---|---|---|---|---|
| `.eyebrow` | `tokens.css:215` | mono | `--text-xs` | `0.06em` | `--muted` | *(unreachable)* |
| `.section-eyebrow` | `style.css:618` | **inherited Inter** | 0.65rem | **0.25em** | `--blue-light` | + `border-left: 2px` |
| `.hero-eyebrow` | `style.css:101` | mono | 0.72rem | 0.1em | `rgba(255,255,255,.60)` |
| `.cap-grid-eyebrow` | `style.css:204` | mono | 0.72rem | 0.1em | `--brand` |
| `.work-rail-eyebrow` | `style.css:374` | mono | 0.72rem | 0.1em | `--brand` |
| `.process-eyebrow` | `style.css:545` | mono | 0.72rem | 0.1em | `rgba(255,255,255,.45)` |
| `.industries-eyebrow` | `style.css:775` | mono | 0.72rem | 0.1em | `--brand` |
| `.trust-block-eyebrow` | `style.css:837` | mono | 0.72rem | 0.1em | `--brand` |
| `.credentials-eyebrow` | `style.css:878` | mono | 0.65rem | 0.12em | `--muted` |
| `.promise-eyebrow` | `style.css:749` | mono | 0.65rem | 0.12em | `--brand` |
| `.cap-mat-label` | `style.css:707` | mono | 0.65rem | 0.1em | `--brand-light` |
| `.footer-tagline` | `style.css:983` | mono | 0.65rem | 0.08em | `rgba(255,255,255,.35)` |
| `.cap-tag` | `style.css:668` | inherited | 0.6rem | **0.3em** | `--brand` |
| `.contact-label` | `style.css:927` | inherited | 0.6rem | 0.2em | `--brand` |
| `.mat-col-title` | `style.css:954` | inherited | 0.6rem | 0.25em | `--blue-light` |
| `.mono-eyebrow` | `style-work.css:36` | mono | 0.75rem | 0.12em | `--color-ember` |
| `.mono-label` | `style-work.css:46` | mono | 0.75rem | 0.08em | `--color-ink-2` |
| `.eyebrow` (scoped ×4) | `gallery.html:35, 150, 204, 295` | mono | 0.75rem | 0.08em | `--brand` / `--brand-light` |

Five distinct sizes (0.6 / 0.65 / 0.72 / 0.75 / `--text-xs`), six tracking values (0.06 / 0.08 / 0.1 / 0.12 / 0.2 / 0.25 / 0.3em), and **four of them are not mono at all**.

The most visible consequence: `.section-eyebrow` — the eyebrow used in the page-hero of **all six sub-pages** (`capabilities.html:33`, `work.html:34`, `gallery.html:364`, `about.html:229`, `contact.html:32`, `404.html:33`) — is the *only* eyebrow in the system that is not IBM Plex Mono and the only one with a `border-left` accent bar. Every eyebrow on `index.html` is mono without a bar. The homepage and the sub-pages use visually unrelated label treatments.

**Fix:** one `.eyebrow` base (mono, `--text-xs`, `--tracking-wide`, uppercase) plus `--brand` / `--muted` / `--on-dark` modifiers. That is 1 class + 3 modifiers replacing 17 classes and the 4 inline overrides in `about.html`.

### 6.6 Pills / tags — 9 variants, 3 of them near-identical

| Class | File:line | Family | Size | Background | Border | Radius |
|---|---|---|---|---|---|---|
| `.work-pill` | `style.css:520` | mono | 0.65rem | `rgba(42,102,159,0.07)` | `rgba(42,102,159,0.20)` | **0** |
| `.cap-spec-pill` | `style.css:684` | **Oswald** | 0.75rem | `rgba(42,102,159,0.07)` | `rgba(42,102,159,0.28)` | **0** |
| `.spec-tag` | `tokens.css:234` | mono | `--text-xs` | `rgba(42,102,159,0.08)` | `rgba(42,102,159,0.22)` | `--radius-sm` 3px |
| `.mat-tag` | `style.css:958` | Oswald | 0.78rem | `rgba(14,165,233,0.15)` | `rgba(56,189,248,0.4)` | 0 |
| `.work-tile__chip` | `style-work.css:216` | mono | 0.7rem | transparent | `--color-rule` | 0 |
| `.work-chip` | `style-work.css:115` | mono | 0.75rem | transparent | `--color-rule` | 0 |
| `.case-process__chip` | `style-work.css:370` | mono | 0.75rem | transparent | `--color-rule` | 0 |
| `.parts-catalog__pill` | `style-work.css:728` | mono | 0.75rem | `--color-bg-1` | `--color-rule` | **999px** |
| `.work-nav__link` | `style-work.css:778` | body | 0.875rem | transparent | `--color-rule` | **999px** |

The first three are the same design idea rendered with three different alpha pairs (`.07/.20`, `.07/.28`, `.08/.22`), three different sizes and two different radii (0 vs 3px). `.mat-tag` is a sky-blue outlier (§2.3). The last two introduce a pill radius (999px) that contradicts every other tag on the site being square.

**Fix:** one `.pill` with `--pill-bg: rgba(brand, 0.08)`, `--pill-border: rgba(brand, 0.22)`, `--radius-sm`, `--text-xs`, `--font-mono`.

### 6.7 Buttons — 7 variants, 3 different geometries

| Class | File:line | Fill | Padding | Size | Radius | Case |
|---|---|---|---|---|---|---|
| `.btn-primary` | `style.css:139` | `--brand` | `0.8rem 2rem` | 0.9375rem | **4px** | sentence |
| `.btn-outline` | `style.css:148` | transparent | `0.8rem 2rem` | 0.9375rem | **4px** | sentence |
| `.nav-cta` | `style.css:64` | `--brand` | `0.6rem 1.5rem` | 0.875rem | **4px** | **UPPER**, 0.05em |
| `.cap-cta` | `style.css:717` | **`#dd5932`** | `0.75rem 2rem` | 0.85rem | **0** | **UPPER**, 0.12em, Oswald |
| `.work-industry-v2__cta` | `style-work.css:1031` | **`#dd5932`** | `0.75rem 2rem` | 0.85rem | **0** | **UPPER**, 0.12em, mono |
| `.case-cta__btn` | `style-work.css:462` | `--color-ember` (broken) | `1rem 2rem` | 0.875rem | **0** | **UPPER**, 0.08em, mono |
| `.cta-band` submit | `contact.html:66` | `--brand` | `0.8rem 2rem` | **0.875rem** (inline) | 4px | sentence |

Two fill colors (`#2a669f` blue and `#dd5932` orange) with no rule governing which is used where — `capabilities.html` and `work.html` get orange CTAs, `index.html`, `gallery.html`, `about.html`, `contact.html` and `404.html` get blue ones. Three padding pairs, four font sizes, two radii, two casings.

**Fix:** `.btn` base + `.btn--primary` / `.btn--ghost` / `.btn--sm`. One radius, one case, one padding scale.

### 6.8 Cards — largely fine

`.work-card` (`style.css:446`), `.industry-tile` (`style.css:794`), `.credential-item` (`style.css:890`), `.shop-stat` (`gallery.html:97`), `.work-industry-v2` (`style-work.css:932`) are all page-local and don't share markup, so there is no cross-page drift to report. Only note: `.work-card` uses `border: 1px solid #E6E8EB` while `.industry-tile` relies on the (broken) grid-gap technique and `.shop-stat` uses `border: 1px solid var(--border)` where `--border` is `rgba(255,255,255,0.10)` — **a 10% white border on a light background is effectively invisible** (`gallery.html:76, 102`). That was presumably meant to be `--rule` / `#D1D9E0`.

---

## 7. BORDER RADIUS, BORDER WIDTHS, SHADOWS, TRANSITIONS

### 7.1 Radius — 4 shipped values, 3 tokens defined, **zero overlap**

| Value | Uses | Lines |
|---|---|---|
| `4px` | 3 | `style.css:68` (`.nav-cta`), `:143` (`.btn-primary`), `:152` (`.btn-outline`) |
| `2px` | 1 | `style.css:1011` (hamburger bars) |
| `50%` | 5 | `style.css:115, 350, 422, 438, 579` |
| `999px` | 2 | `style-work.css:733, 785` |

Tokens define `--radius-sm 3px`, `--radius-md 6px`, `--radius-lg 12px`. **Not one is used, and no shipped value matches any of them.** Meanwhile the pill components use square corners on `index.html` (`.work-pill`, `.cap-spec-pill`) but fully-round on the work pages (`.parts-catalog__pill`, `.work-nav__link`).

**Fix:** set `--radius-sm: 4px` (matching what actually ships), `--radius-md: 8px`, `--radius-pill: 999px`, `--radius-round: 50%`. Then decide: square pills or round pills, site-wide.

### 7.2 Border widths — consistent, the one clean axis

`1px` dominates (67 declarations); `2px` is reserved for accent marks (`style.css:63` active nav underline, `:580` timeline node, `:618` eyebrow bar, `:760` `.ind-rule`, `:806` `.industry-mark`, `:956` `.mat-col-title` bar, `:1011` hamburger; `style-work.css:914, 999, 1055` note/badge bars). No 3px, no 0.5px. **No action needed** beyond routing both through `--border-width` / `--border-width-accent`.

### 7.3 Shadows — 4 distinct, none matching a token

| Shadow | File:line | Component |
|---|---|---|
| `0 1px 8px rgba(13,43,78,0.07)` | `style.css:43` | nav |
| `0 4px 16px rgba(13,43,78,0.10)` | `style.css:1038` | mobile nav dropdown |
| `0 8px 32px rgba(13,43,78,0.12)` | `style.css:456` | `.work-card:hover` |
| `0 8px 40px rgba(0,0,0,0.18)` | `style.css:663` | `.cap-content` |

`tokens.css:129–136` defines `--shadow-sm/md/lg/xl` — **all four unused**. The four shipped shadows are close to but not equal to the tokens (`--shadow-sm` is `0 1px 3px … , 0 1px 2px …`; the nav uses a single-layer `0 1px 8px`). The fourth is the only one built on pure black rather than navy, so it reads slightly greyer than the other three.

**Fix:** four shadows for four elevations is right — just make them the four tokens, and rebase `style.css:663` onto `rgba(8,27,51,…)`.

### 7.4 Transitions — 10 literal durations, 2 competing token sets, 0 tokens used

| Literal | Uses |
|---|---|
| `0.2s` (200ms) | 12 (incl. 3 as `0.2s, border-color 0.2s`) |
| `0.25s` | 2 |
| `0.3s` | 5 (incl. one with `0.05s` delay) |
| `0.4s` | 2 |
| `0.5s` | 1 |
| `0.6s` | 3 |
| `0.65s` | 1 |
| `1.2s` | 1 |
| `7s` | 1 (`style.css:651`, `.cap-bg img` scale) |
| `380ms` | 1 (`style.css:601`) |

`tokens.css` offers **two mutually inconsistent duration sets**: v2 `--duration-fast 150ms / --duration-base 300ms / --duration-slow 500ms` and v3 `--dur-fast 120ms / --dur-base 220ms / --dur-slow 420ms`. Neither is used; the site's de facto standard is `0.2s`, which matches neither.

Easing: `ease` (default) 8×, `ease-out` 1×, `cubic-bezier(0.4,0,0.2,1)` 2×, `cubic-bezier(0.22,1,0.36,1)` 2× (as a `var()` fallback), plus the 10 broken `var(--ease-out)` / `var(--ease-out-3)` references. Five easing tokens defined, zero reachable.

`0.25s` vs `0.3s` and `0.6s` vs `0.65s` are perceptually identical pairs.

**Fix:** pick one set — `--dur-fast 150ms`, `--dur-base 250ms`, `--dur-slow 400ms` — delete the other, and route all 10 literals through them. Keep `7s` as the one deliberate exception (slow Ken Burns).

---

## 8. DEAD CSS

### 8.1 `style-work.css` is loaded by 2 of 7 pages and used by 1

| Page | Loads `style-work.css` | Classes actually used from it |
|---|---|---|
| `capabilities.html:10` | **yes** | `nav-cta`, `nav-logo` only — and both are defined in `style.css`; the `style-work.css` versions are all scoped under `body.work-theme`, and **no page has `class="work-theme"` on `<body>`** (verified on all 7). **So `capabilities.html` uses zero rules from this file.** |
| `work.html:11` | yes | 25 classes — `work-industry-v2*`, `work-visual-strip*`, `case-cta*`, `mono-eyebrow`, `mono-label` |
| other 5 | no | — |

**`capabilities.html:10` loads 28KB of CSS that has no effect. Remove that line.**

Of `style-work.css`'s 122 class selectors, **97 are unused by the 7 audited pages**. Of those 97, only 7 (`case-hero`, `case-numbers`, `case-photos`, `case-process`, `case-section`, `fill-todo`, `work-theme`) are used by the `work/` subdirectory. **The remaining 90 are dead everywhere in the repository**, including these complete component families:

- **`.matrix*`** — 12 selectors, ~150 lines (`style-work.css:496–654`). A full Material × Process matrix component: table, sticky headers, tooltips, controls, legend. Zero markup uses it.
- **`.parts-catalog*`** — 13 selectors, ~85 lines (`style-work.css:669–751`).
- **`.work-tile*`** + `.work-grid` + `.work-filter*` + `.work-chip` + `.work-hero*` — 22 selectors, ~200 lines (`style-work.css:56–247`). This is the original `work.html` design, superseded by `.work-industry-v2` at `style-work.css:932+` but never removed. **`work.html` therefore ships two complete, mutually exclusive designs for the same page.**
- **`.work-industry*`** (v1, non-`-v2`) + `.work-nav*` — 16 selectors, ~130 lines (`style-work.css:754–929`). A *third* design for the same page.

Roughly **660 of 1145 lines (58%) of `style-work.css` is dead.**

### 8.2 Dead in `style.css` — 42 of 165 selectors

Excluding JS-applied state classes (`.is-visible`, `.open`, `.active`, `.animated` — these are added at runtime and are **not** dead), the genuinely unused selectors are:

| Family | Selectors | Lines | Notes |
|---|---|---|---|
| `.mat-*` | `.mat-section`, `.mat-bg`, `.mat-content`, `.mat-col-header`, `.mat-heading`, `.mat-col-title`, `.mat-tags`, `.mat-tag` (8) | `style.css:928–967` + mobile `1113–1124` | Superseded by `.cap-mat-grid`. Holds the entire sky-blue stray palette (§2.3). |
| `.promise-*` | `.promise-section`, `.promise-inner`, `.promise-eyebrow`, `.promise-grid`, `.promise-item`, `.promise-icon`, `.promise-text` (7) | `style.css:744–753` + `853, 862–864, 1090, 1093–1094, 1170` | **`.trust-block .promise-grid` at `style.css:854` and the mobile rules at `862–864` reference a component that no longer exists inside `.trust-block`** — `index.html:382–428` contains only credentials. |
| `.ind-*` | `.ind-grid`, `.ind-card`, `.ind-name`, `.ind-desc`, `.ind-rule` (5) | `style.css:753–761` | Superseded by `.industries-grid` / `.industry-tile`. Note `.ind-desc` is defined **twice** (`:759` and `:761`) with conflicting colors. |
| `.process-list` legacy | `.process-list`, `.process-item`, `.process-num`, `.process-desc` (4) | `style.css:605–613` | Superseded by `.process-timeline`. **`.process-title` at `style.css:612` belongs to this dead family and is actively breaking the live component — see §8.4.** |
| `.credentials-bar` | `.credentials-bar`, `.credentials-inner`, `.credentials-eyebrow` (3) | `style.css:868–883` | Superseded by `.trust-block`; the inner items (`.credential-item` etc.) are still live. |
| `.photo-grid` | `.photo-grid`, `.photo-grid img`, `.span-2` (3) | `style.css:732–744`, `1128–1134`, `1175–1176` | Superseded by `.curated-grid` in `gallery.html`'s embedded styles. |
| `.section*` | `.section`, `.section-inner`, `.section-title`, `.section-body` (4) | `style.css:616–620`, `1137` | Only `.section-eyebrow` survives. `.section` also **collides with `tokens.css:261`** (`padding-block: var(--section-gap)` vs `padding: 5rem 3rem`) — a name conflict that will bite the moment tokens are loaded. |
| Misc | `.gallery-bg`, `.gallery-header`, `.trust-divider`, `.work-rail-dot`, `.cap-tile--wide`, `#capabilities` (6) | `style.css:729–730, 848, 437, 278, 1097` | `.cap-tile--wide` is commented `(unused, kept)` — accurate. `.work-rail-dot` markup was removed but the JS carousel may still reference it. |

Roughly **300 of 1209 lines (25%) of `style.css` is dead.**

### 8.3 Selector-name collisions across files

Once `tokens.css` is correctly loaded, three selectors will collide:

| Selector | `tokens.css` | `style.css` / page | Conflict |
|---|---|---|---|
| `.section` | `:261` `padding-block: var(--section-gap)` | `style.css:616` `padding: 5rem 3rem; background: var(--white)` | style.css wins (later); token version silently lost |
| `.eyebrow` | `:215` mono / `--text-xs` / `0.06em` / `--muted` | `gallery.html:35,150,204,295` scoped overrides at 0.75rem / 0.08em / `--brand` | 4 page-local overrides of a global class |
| `.reveal` | `:248` `translateY(24px)`, `var(--anim-duration)` 380ms, `var(--ease-out)` | `style.css:601` `translateY(20px)`, literal `380ms ease-out` | Different distance (24px vs 20px) and different easing. Used 4× on `index.html` only. |

### 8.4 The dead-CSS bug: `index.html`'s process heading renders at 14px

This is the single highest-impact finding after the tokens issue.

```css
/* style.css:551 — the live component */
.process-title {
  font-family: 'Inter', sans-serif;
  font-size: clamp(1.75rem, 3vw, 2.5rem); font-weight: 600;
  letter-spacing: -0.025em; font-optical-sizing: auto;
  color: #ffffff; line-height: 1.15;
}

/* style.css:612 — inside the "Legacy process list (unused but kept for safety)" block */
.process-title { font-size: 0.9rem; font-weight: 600; margin-bottom: 0.25rem; }
```

Same selector, same specificity, **line 612 wins**. `index.html:336` — `<h2 class="process-title">From First Contact to Final Part.</h2>` — the section heading for the process timeline renders at **0.9rem (14.4px)** instead of the intended **28–40px**. It is currently smaller than the body copy beneath it and barely larger than its own 0.72rem eyebrow. The comment "unused but kept for safety" is exactly wrong: keeping it is what broke the page.

Deleting `style.css:605–613` fixes it.

---

# TOP 10 FIXES

Ranked by visual impact per unit of effort.

| # | Fix | Effort | Impact |
|---|---|---|---|
| **1** | **Delete `style.css:605–613`** (the legacy `.process-list` block). `.process-title` at `:612` is overriding the live rule at `:551`, rendering `index.html`'s process section heading at **14.4px instead of 28–40px**. | **1 line-range delete** | A homepage section heading currently looks broken. Highest impact-to-effort ratio on the site. |
| **2** | **Load `tokens.css` on all 7 pages** (`<link rel="stylesheet" href="tokens.css">` before `style.css`), then delete the shadow `:root` at `style.css:3–32`. | 7 link tags + 1 block delete, then a careful regression pass | Fixes the **26 undefined variables** in one move. Repairs `work.html`'s invisible CTA button, its zero desktop side-padding, its missing section dividers, and `index.html`'s dead work-card hover. Unblocks every other token fix below. |
| **3** | **Split `--rule` into `--rule-color` and `--rule-border`** and correct the 11 invalid call sites (`style.css:747×2, 750, 791, 792, 826, 827, 851, 855, 870, 871, 913`). | ~15 line edits | Restores `index.html`'s **industries grid lines** (currently invisible), the **trust-block section rules**, and the **5 credential dividers**. Three visible structural elements on the homepage return. |
| **4** | **Delete the 7 inline `style="height:36px;"` attributes** on `.nav-logo img` (`index.html:15`, `capabilities.html:16`, `work.html:17`, `gallery.html:347`, `about.html:211`, `contact.html:15`, `404.html:16`). | 7 attribute deletes | The CSS is already correct at `style.css:49` / `:1045`. This single change makes the **mobile nav logo shrink to 28px on all 7 pages** — currently it never does, because inline styles outrank media queries. |
| **5** | **Remove Oswald** — rewrite the 6 rules at `style.css:630, 673, 688, 721, 949, 963` to Inter 600 with corrected tracking. | 6 rules | Oswald is **never loaded** by any page. Fixes the H1 on `work.html`/`gallery.html`/`about.html` and **every heading, all 68 spec pills, and the CTAs on `capabilities.html`**, all of which currently render in a wide fallback grotesque with condensed-face tracking. |
| **6** | **Unify `.page-hero h1`** — set `style.css:629–636` to Inter/600/`-0.025em`, delete the 3 inline overrides (`capabilities.html:34`, `contact.html:33`, `404.html:34`) and the `about.html:228` width wrapper; add a shared `.page-hero__inner`. | ~6 edits | Six sub-page headers currently split into two visually unrelated treatments (Inter 600 / −0.025em vs fallback 700 / +0.03em), and one page indents differently. This is the first thing a visitor sees on every non-home page. |
| **7** | **Replace `work.html`'s `.case-cta` block (lines 171–177) with the standard `.cta-band` / `.btn-primary` markup**, and delete `<link href="style-work.css">` from `capabilities.html:10`. | ~8 lines + 1 delete | Gives all 7 pages one CTA component instead of two, removes a broken transparent button, and drops 28KB of no-op CSS from `capabilities.html`. |
| **8** | **Collapse the navy cluster** — pick `#0D2B4E` (surfaces) + `#081B33` (footer/scrims); delete `#141c2e`, `#1b2540`, `#243050`, `#1e2d3d`, `#0E2954` and convert the 21 raw `#0D2B4E` uses plus `style-work.css:943–946`. | ~40 find-and-replace edits | Removes **five near-identical dark navies** and fixes the three-different-heading-colors problem across `index`/`work`/`gallery`. This is the change that makes the site read as one brand rather than three iterations. |
| **9** | **Extract the `contact.html` form to classes** — `.form-row`, `.form-field`, `.form-label`, `.form-input`, `.btn--block`. Removes 10 of the 18 inline styles on that page, plus the fragile `.cta-band form > div:first-of-type` selector at `style.css:1156`. Add `.footer-logo` to kill 7 more inline blocks. | ~30 lines of new CSS, ~20 attribute deletes | Eliminates 34 of the 52 inline styles site-wide and 5 hardcoded literals repeated 4–5× each. Pure maintainability, but it is what stops the drift from recurring. |
| **10** | **Consolidate the eyebrow and pill components** — one `.eyebrow` (mono / `--text-xs` / `--tracking-wide`) with `--brand`/`--muted`/`--on-dark` modifiers replacing 17 classes; one `.pill` replacing `.work-pill`/`.cap-spec-pill`/`.spec-tag`. Delete `.mat-*` (8 selectors, the sky-blue strays) and the 90 dead `style-work.css` selectors while you are in there. | ~2 hours | Fixes the homepage-vs-subpage label mismatch (`.section-eyebrow` is the only non-mono eyebrow and the only one with a border bar), removes the three-alpha pill inconsistency, and deletes ~960 lines of dead CSS (58% of `style-work.css`, 25% of `style.css`). |

### Quantified summary

| Axis | Current | Coherent target |
|---|---|---|
| Color literals | **73** (27 hex + 46 rgba) + 27 unreachable tokens | ~12 base + one 5-step alpha ramp |
| Font sizes | **51** (35 fixed + 16 clamp) | 7 (4 fixed + 3 clamp) |
| Letter-spacing | **21** | 4 |
| Line-height | **17** | 4 |
| Spacing values (rem) | **35** (11 on-scale, 24 off, 13 off even the 4px grid) | 13 |
| Border radii | **4** shipped, 0 matching the 3 defined tokens | 4, tokenised |
| Shadows | **4**, none matching the 4 defined tokens | 4, tokenised |
| Transition durations | **10** literals + 2 competing token sets | 3 |
| Eyebrow variants | **17** | 1 + 3 modifiers |
| Button variants | **7** (2 fill colors, 3 paddings, 4 sizes, 2 radii, 2 casings) | 1 + 3 modifiers |
| Pill variants | **9** (3 near-identical) | 1 + 2 modifiers |
| Inline `style=""` | **52** (34 in repeated identical blocks) | ~4 (per-image art direction only) |
| Dead selectors | **139** (42 in `style.css`, 97 in `style-work.css`) | 0 |
| Dead CSS lines | **~960 of 2354 (41%)** | 0 |
| Token utilisation | **0%** | — |

No files were modified.
agentId: a92ac6070a9863880 (use SendMessage with to: 'a92ac6070a9863880', summary: '<5-10 word recap>' to continue this agent)
<usage>subagent_tokens: 175634
tool_uses: 37
duration_ms: 679710</usage>