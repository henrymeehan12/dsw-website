# DSW website rebuild — project log & handoff

**Last updated:** 2026-08-18
**Purpose:** pick this up in a fresh session without re-deriving anything.

---

## 1. Where everything lives

| Thing | Location |
|---|---|
| Source repo | `github.com/henrymeehan12/dsw-website` (public) |
| Local clone | `C:\Users\tyler\source\repos\dsw-website` |
| Hosting | Netlify project **dswcutting**, auto-deploys from `main` |
| Staging URL | `https://dswcutting.netlify.app` |
| Live (old) site | `https://dswcutting.com` — WordPress, built by **Infomedia**, hosted at `50.116.39.119` |
| DNS | Wix account `h.meehan@dswcutting.com` → Domains → ⋯ → Manage DNS Records |
| Registrar | Network Solutions. Registrant: Chris McIlvaine. Renews **Nov 5, 2026** |

**Goal:** replace dswcutting.com with the new site. Chris McIlvaine's sign-off is required before the domain moves.

**Site structure:** 8 reachable pages — `index`, `capabilities`, `work`, `gallery` (nav label "The Shop"), `about`, `contact`, `thank-you`, `404`. Plus 5 case-study pages under `work/` that still contain **15** `[FILL: …]` placeholders (the earlier count of 22 was wrong — 15 is the verified count across the 5 files). **They are no longer deployed** — see §3 Session 3.

**Stylesheets:** `tokens.css` → `style.css` → `style-work.css` (last one only on capabilities + work). Load order matters — see §4.

---

## 2. Established facts (corrected 2026-08-18 — use these, not older copy)

- **Multiple buildings**, not one. Never say "one roof" or "single facility." The approved framing is **"One company. One point of contact."**
- **230,000 sq ft** = combined across Birmingham facilities.
- **8 flat fiber lasers** (7 Mitsubishi + 1 Bescutter Giga 20 kW) + **3 tube lasers** = **11 fiber lasers total.** (About stat band said 12 until session 4 — now 11.)
- **3 material towers feed 6 of the 8 flat lasers** (was published as 2 towers / "five automation-fed" until session 4).
- **Robotic welding: 4 robots across 9 automated weld windows** (three 2-window cells + one 3-window cell). Publish the totals only, never the per-cell breakdown.
- **Site policy since 2026-09-03 (Chris review): no machine manufacturer or model names in copy** — chips, body, headings, meta, JSON-LD. Specs only. Brands are fine in photo filenames, `alt` text and `og:image`. **FARO is the one exception** and stays everywhere (customers name it in their own quality requirements). The brand facts in this section are for internal reference only.
- **No history / heritage / city-lore / sentiment on the site**, and **no named individuals** (Chris's name is off the site by his own request). Company voice only.
- **Do not advertise short-notice / rush / pull-ahead delivery**, even though the shop can do it.
- **Tube lasers are: BLM LT8, Bescutter HyTube 7532, Bescutter Titan 12036D.** The **Bescutter Apex 3030 has been removed** from the site — it is not in service.
- 2024: dedicated tube-laser facility opened in North Birmingham, 3 machines.
- Other constants: founded 2004 · 80 employees · 2 plasma tables · 5 Mitsubishi press brakes + 1 robotic bending cell · 4 OTC Daihen weld robots · 2 Haas VF-3YT-50 machining centres (recently installed) · 6 trucks · 2″ max plate · 40 ft max tube outbound · 14″ max OD.
- **Finishing (paint, galvanising, plating, e-coat) is via partner vendors, NOT in-house.** Word it as "managed" or "coordinated" — never "in-house."
- **Customer confidentiality:** kingpin nest work cannot be shown. Rule of thumb — a specific part name plus a specific annual volume identifies a program even with no customer named. Use shop-wide numbers if numbers are wanted.
- Removed claim: "the deepest tube capability in the Southeast" (unsupportable superlative).

---

## 3. Done so far

### Session 1 — "Real Parts. Real Programs." rail (deployed, commit `c3ae22e`)
- Cut the homepage featured-work rail from 5 placeholder cards to **4 program cards**: New Part Launch · Frame Rails & Structural Members · Stocking Program · Complete Weldments.
  - Reasoning: 4 of 6 originally-proposed themes duplicated the "What We Do" grid one scroll above. Cards must be **parts or programs**; capabilities become the *proof inside* them.
- Carousel JS hides arrows/dots when there's nothing to scroll (`VISIBLE = 4`, so 4 cards = 0 slides).
- Added a 2-line `min-height` on `.work-card-title` so cards stay aligned.
- Added **Construction & Infrastructure** and **Material Handling** sections to `work.html` (homepage advertised 7 industries, Work page had 5).
- Added `_redirects` — 14 rules mapping all 7 legacy WordPress URLs. `/careers/` is a **302 on purpose** (no careers page yet).
- Added `404.html`.

### Session 2 — full audit + P0 fixes (in the local clone, **not yet committed**)
Full audit report: `AUDIT-FINDINGS.md`, with sub-reports `copy-report.md` and `design-report.md`.

Fixed:
1. **`.process-title` was rendering at 14.4px** — a legacy `.process-list` block in `style.css` was overriding it. Deleted. Now 40px.
2. **Oswald removed** — it was set in 6 CSS rules but never loaded by any page. Headings → Inter 600; spec pills → IBM Plex Mono (matching `.work-pill`).
3. **Anchor IDs added to `capabilities.html`** — `laser, tube, forming, machining, welding, secondary, tooling, finishing, engineering, materials`. The 5 homepage capability tiles previously linked to IDs that didn't exist.
4. **`--rule` bug fixed** — it's a *border shorthand*, but 11 sites used it as a colour (`1px solid var(--rule)` → invalid). Those now use `--dark-border`. This restored the credential dividers, industries grid lines and trust-block rules, all previously invisible.
5. **`tokens.css` now linked on all 7 pages**, before `style.css`. It was never loaded — 26 custom properties on capabilities/work resolved to nothing.
6. **Nav consistency** — work.html + 404.html nav CTA "Get a Quote" → "Contact Us"; contact.html nav CTA gets `active` + `aria-current`.
7. **Meta descriptions on all 6 pages that lacked them**, plus Open Graph + Twitter card + canonical tags site-wide (404 excluded, it's `noindex`).
8. **Fact corrections** per §2 across index, capabilities, gallery, about.
9. **Terminology unified** — "Press Brake & Rolling" everywhere (was Forming / Rolling / Roll).
10. **Inch marks** `"` → `″` in capabilities.html body copy.
11. **Contrast fixes** — credential label/sub `#94a3b8` → `#556575` (2.39 → 5.58); timeline step numbers `--brand` → `#7FB6E5` (2.38 → 6.6). **index.html low-contrast count went 15 → 0.**
12. **Credential row top-aligned** (`align-items: flex-start`) — one- vs two-line sub-text was shifting items out of line.
13. Acronyms expanded on homepage cards: FAIR → First-Piece, VMI → Managed Inventory.

**Verified after changes:** no JS errors, no broken images, no mobile horizontal overflow, Oswald gone from all pages, all 10 anchors resolve, no stale claims remain.

### Session 3 — thank-you page, case-study quarantine, two capabilities sections (in the local clone, **not yet committed**)

1. **`thank-you.html` built.** Matches the `404.html` pattern (page-hero + `cta-band` + contact-row), `noindex, follow`. `contact.html`'s form now carries `action="/thank-you"`, so Netlify posts land on a branded page instead of Netlify's generic success screen.
   - **The two `/thank-you` rules were deleted from `_redirects`**, not repointed. A redirect rule pointing at a path that is now a real file is a trap for the next person; the static file plus `pretty_urls` already resolves `/thank-you`, `/thank-you/` and `/thank-you.html`.
2. **The 5 case-study pages are stripped from the deploy.** `netlify.toml` gained `command = "rm -rf work"` under `[build]`. The source stays in git; the build output does not contain them, so they are neither live nor indexable. **Deleting that one command line is the whole re-publish step** once the 15 placeholders are filled. Nothing on the site links to `work/*.html`, so nothing breaks.
   - Rejected alternative: forced 404 rules in `_redirects` (`/work/* … 404!`). With `pretty_urls = true`, `/work` and `/work/` are ambiguous against `work.html`, and a mis-scoped rule would have taken out the Work page.
3. **`capabilities.html` gained `#quality` and `#delivery` sections** — the last two homepage tiles that pointed at content which didn't exist. Both are process-based; the only equipment named is what the homepage tiles and their photos already publish (FARO arm CMM, laser scanner, 2 flatbeds + 4 Conestogas). Nothing new was claimed.
   - The Quality tile now goes to `capabilities.html#quality` (was the top of the page); the Delivery tile goes to `capabilities.html#delivery` (was `contact.html`).
   - Section order and the `text-left` alternation were preserved: engineering → quality (`text-left`) → delivery → materials (`text-left`).
4. **Heading order fixed on `capabilities.html`** — all 12 `.cap-heading` elements went `h3` → `h2`, closing the h1→h3 skip. No CSS targets `h3` as an element selector, so this is visually identical. Sequence is now `H1 H2 ×12`.

**Verified after changes:** all 6 anchors referenced from `index.html` resolve on `capabilities.html`; no console or page errors (the only network failures are Google Fonts, blocked by the sandbox, not a site fault); no horizontal overflow on capabilities or thank-you at 390px; `netlify.toml` parses as valid TOML.

**Known and accepted:** the new sections inherit the existing `.cap-cta` orange-on-white contrast failure (3.78:1). It is the same button used by the other ten sections — fix it once, globally, as part of the P1 contrast pass rather than diverging here.

---

### Session 4 — Chris review pass (2026-09-03, spec: `dsw-site-edit-spec.md` §1–§6; in the local clone, **not yet committed**)

1. **All machine branding stripped from copy** on `capabilities`, `index`, `gallery`, and the unlinked `work/frame-rail-components.html` + `work/heavy-structural-weldments.html`. Chips became spec-only (`8× Flat Fiber Lasers`, `20 kW · Large Frame · 11.5 ft Wide`, `3D Bevel Head · 0–45°`, `2× HD Plasma Tables`, `400 A Plasma · Up to 2″ Plate`, `3× Tube Laser Systems`, `5× CNC Press Brakes`, `Plate Roller`, `2× CNC Vertical Machining Centers`). The CNC body lost the 40-taper comparison and the "specs will publish as the cells come online" line. Siegmund tables → "modular welding tables" on the work page; BLM chip → `40 ft Outbound`. Hits left in `<img alt>` / `og:image` / filenames are intentional.
2. **Towers → 3** (`3× Automation Towers`, "six of them tower-fed"; homepage automation tile: `3 towers feeding 6 lasers`).
3. **Welding publishes both numbers**: `4× Welding Robots` + `9 Automated Weld Windows` chips, body "one window loading while another runs"; homepage tile `4 robots · 9 automated weld windows · manual MIG & TIG`; gallery step 04 matched.
4. **Local Delivery**: `Short-Notice Pulls` chip deleted, body rewritten per spec ("on the day they're scheduled"). Repo-wide `rush|short.notice|pull-ahead|expedite|quick.turn` = 0 hits.
5. **About page cut**: "A Shop Reborn in Steel City" text and the entire "Steel has always run through this city" heritage banner (incl. `plasma-burst-dark.jpg` block) deleted. Hero photo kept with a 3-line plain-fact intro in its place. Stat band verified standalone with its white background — and corrected **12 → 11**. Meta/og description replaced. Dead `.heritage-banner*` and `.leadership-*` CSS removed (no leadership block ever existed in the HTML). Chris's name: 0 hits site-wide.
6. **De-flowering**: index hero subhead, all five section heads, closing CTA, and the partner section (now "How We Work", 1 short paragraph — the About page keeps the long "Product Partner" version) per the spec table. The process section's eyebrow was dropped because it already read "How It Works" and would have doubled the new title. Capabilities: Jig & Fixture closing sentence and "No handoffs. No finger-pointing." removed. `contact.html` shared the index closing line verbatim, so it got the same "Send Us Your Drawings." replacement. Gallery facility paragraph lost its "industrial corridor … built American infrastructure" line (city-lore) — now a plain 230,000 sq ft / two shifts statement.

**Verified after changes (§8 checklist, run against a copy of the built output = repo minus `work/`):** brand list → 0 hits outside `<img>`/`og:image`; `McIlvaine|Chris` 0; rush/short-notice 0; `Sloss|knife shop|1960s|Steel City` 0; the `work/` pages pass the same greps; every `capabilities.html#…` anchor still resolves; no `about.html#` anchors exist anywhere; About `<div>` balance 44/44; rendered screenshots of about/index/capabilities look right.

**Spec item that could not be satisfied as written:** §8 says "FARO still present on … About." FARO was never on the About page (0 hits at `67ab6d0`), so nothing was removed — it just was never there. Adding it is a one-word change to differentiator 02 if wanted; not done because it wasn't in §1–§6.

### Session 5 — "not a job shop" pass, Work page retired, CNC labelled (2026-09-03, **not yet committed**)

Henry's framing: DSW has a few large program customers, not many small ones. The site should get a buyer onto a call, not collect drawings from anyone. **No file upload on the contact form — ever.**

1. **CTA / tone pass.** Homepage step 01 "Get a Quote / Send your drawings" → "Start a Conversation" (talk to a sales engineer about what you're building, volumes, how the program would run). Closing band on index → "Let's Talk About Your Program. / Tell us what you're building and we'll set up a call." Contact page: h2 "Tell Us About Your Program.", phone called out as fastest (now a `tel:` link, email a `mailto:`), message field relabelled "What are you building, and at what volume?", meta/og description rewritten. About + gallery CTA subline, thank-you body ("if it's easier to talk it through, call…"), thank-you "Send Drawings" label → "Email", 404 body + button ("Get a Quote" → "Contact Us") all brought in line. The remaining "drawing"/"quoting" mentions (About §Product Partner, capabilities Engineering + Quality) are the DFM-at-quote sales argument and stay on purpose. Netlify form is still named `quote-request` — renaming it would register a new form in the dashboard; cosmetic, left alone.
2. **Work page retired.** `work.html` deleted from git (history keeps it at `353e7b8`). Reasoning: customer anonymity + not showing competitors the parts + Chris's "what we do could change tomorrow" all point away from a parts page; program credibility already lives in the homepage rail. Nav and footer "Work" links removed on all 7 pages; 404 and thank-you lost their Work tiles; homepage rail eyebrow "Featured Work" → "Programs", "See All Work →" removed; the four rail cards now deep-link to `capabilities.html#quality / #tube / #delivery / #finishing`. Industries section got `id="industries"` (and lost its duplicate eyebrow); `_redirects` sends `/work` and `/work.html` → `/#industries` (301). `capabilities.html` no longer loads `style-work.css` (it used none of it). The five `work/` case-study files stay in git, still stripped from the build.
3. **CNC not operational** (training starts week of 2026-09-07). Chip "Recently Installed" → **"Coming Online Q4 2026"**, body ends "Installed and being commissioned — coming online Q4 2026." Homepage Complete Weldments card no longer claims "machined" on a shipped part (pill "CNC Machined" → "Weld-Ready"). "Machining" stays in the service lists (hero subhead, meta, About meta) because the equipment is on the floor and the date is published. **Photo still the `cap-coming-soon.jpg` placeholder — Henry is supplying a real one.**

**Verified:** every internal `href` and `#anchor` across the 7 deployed pages resolves (scripted check); brand / name / rush / heritage greps still 0 in copy; no `work.html` references anywhere; screenshots of index, contact, 404, thank-you reviewed.

### Session 6 — SEO basics (2026-09-03, **not yet committed**)

1. `sitemap.xml` (5 indexable pages; thank-you and 404 excluded) and `robots.txt` (allow all, disallow `/thank-you`, sitemap link).
2. **`LocalBusiness` JSON-LD** in the `<head>` of the 5 indexable pages: name, address, phone, email, founded 2004, hours, service area, Instagram/LinkedIn `sameAs`, and a 9-item service catalog that deep-links to the capabilities anchors. No equipment brands, no counts, nothing that could go stale except hours.
   - **Hours are Mon–Fri 07:00–16:30, taken from the public listing that mirrors Google** — **Henry to confirm.** If the front office keeps different hours from the floor, publish the office hours.
3. **Canonical / og:url / sitemap all use `https://www.dswcutting.com/`** (was apex). This matches the cutover plan below — www is the primary domain, every indexed legacy URL is www — so the canonical tag won't point through a redirect. If the primary-domain decision ever flips to apex, change these in the same commit.

**After cutover (not before):** claim/verify the site in Google Search Console for `www.dswcutting.com`, submit the sitemap, and make sure the Google Business Profile shows the same name, address, phone, hours and website as the schema. Netlify's primary-domain setting must be www for the canonicals above to be right.

### Session 7 — CNC photos (2026-09-03, **not yet committed**)

Henry shot the machining centers (9504×6336 camera originals). Originals are archived in **`_originals/`** (gitignored — never committed, never deployed; the folder lives only on Henry's PC, so back it up with the rest of the camera roll). Web copies made with Pillow (Lanczos, JPEG q82, progressive):
- `photos/cnc-probe-hero.jpg` — 2000 px, 104 KB — the capabilities `#machining` background (replaces `cap-coming-soon.jpg`).
- `photos/cnc-vmc-front.jpg` — 1600 px, 231 KB — full-width tile at the end of the second gallery grid.
Machine badge and logo are visible in both; that's allowed (brands in photos stay). Alt text describes the picture without naming the brand. **Swap for in-use shots once production starts** — one `src` change each.

### Session 8 — photo pipeline (2026-09-03, **not yet committed**)

Reality check first: only 9 of the 36 images in use were oversized (2400–2560 px cap-section backgrounds); the rest were already 1280 px / ~100 KB. The heavy items were those 9 and the 6.3 MB hero video.
1. **9 backgrounds resized to 2000 px** (Lanczos, JPEG q82, progressive). Pre-resize copies in `_originals/web-src/`. Judged on a 1:1 crop of a 1920-wide render — no visible difference. Saved ~1.2 MB.
2. **Hero video re-encoded**: same 1280×720 / 24 fps, H.264 CRF 27, no audio track, faststart → `photos/dsw_webloop.mp4`, **6.3 MB → 3.2 MB**. Original in `_originals/web-src/`.
3. **`loading="lazy" decoding="async"`** on every photo `<img>` except the first one on each page (index tile 1, capabilities laser background, about/gallery lead photo). Gallery already had it.
4. **72 unused photos (14 MB) moved out of `photos/` into `_originals/unused/`** (gitignored). `git` shows them as deleted; they're still in history and on Henry's PC. `photos/` is now 36 files / 8 MB, every one referenced.
5. Not done, worth knowing: three capabilities backgrounds (`dsw-conestoga-loading`, `faro-arm-inspection`, `worker-hands-parts`) are only **1280 px** and get stretched full-bleed on a 1920 screen — the opposite problem. If higher-res originals exist, drop 2000 px versions in with the same filenames.
6. **Homepage Laser & Plasma tile photo swapped** for Henry's new laser-head shot (`photos/laser-head-sparks.jpg`, 2000 px / 191 KB, `object-position: center 55%` so the head sits in the tall crop; original `_originals/laser-head-DSC04330.JPG`). `mitsubishi-laser-sparks.jpg` stays only because it is every page's `og:image`.
7. **Homepage capability grid fixed (`style.css`)** after Henry spotted the Quality tile shorter than the Local Delivery tile. Cause: the standard-tile `aspect-ratio: 16/10` rule out-specified `.cap-tile--wide2`, so the two-column Delivery tile was 16/10 across two columns. Fix: exclude `--wide2` from that rule and give it `aspect-ratio: 2.56` (= 2 / (1.25 × 10/16)), which makes both third-row tiles the same height. Also dropped the tall tile's `min-height: 560px`, which was forcing rows 1–2 taller than their tiles and opening white gaps beside them.
   - **Found while testing: on phones (≤768 px) four of the seven tiles were invisible** — the desktop `grid-column: 2 / 4` / `grid-row: 1 / 3` spans created an implicit zero-width third column in the 2-column mobile grid and Laser, Tube, Welding and Automation landed in it. Mobile is now a single column, every tile 16/10, verified at 390 and 768 px. This was live on the netlify site.
`netlify.toml` image compression stays **off** — we control quality here.

### Session 9 — mobile pass (2026-09-03, **not yet committed**)

Every page rendered at iPhone 13 (390 px) and iPhone SE (375 px) after the grid fix. No horizontal overflow on any page. Fixed:
1. **Hero eyebrow** ("Founded 2004 · Birmingham, AL · 230,000 sq ft") broke mid-item into three ragged columns — now wraps whole items (`flex-wrap`, `white-space: nowrap` per span).
2. **Homepage stats bar** was four-across on phones with labels at ~7 px — now a 2×2 grid at readable sizes (the ≤480 px override that shrank it further is gone).
3. **Gallery copy** still said the machining centers "add CNC machining to the floor" — now "are installed and come online Q4 2026", matching capabilities.
Checked and fine: capabilities sections stack photo-over-card; About and Contact clean; nav logo / Contact button / hamburger don't collide down to 375 px; work rail stacks vertically with controls hidden. (Blank photo areas in full-page screenshots are lazy-loading not firing in the headless capture — images load normally on scroll.)

### Session 10 — capabilities sheet PDF (2026-09-03, **not yet committed**)

`DSW-Capabilities-Sheet.pdf` (Letter, 2 pages, ~450 KB) at the repo root, linked from the capabilities page hero and the Quality section. **Built from the live capabilities copy** (chips + body text scraped from `capabilities.html`), so it says exactly what the site says: no brands, CNC "coming online Q4 2026", no certifications claimed. Source is `_tools/capabilities-sheet.html` (self-contained, fonts from Google, photos inlined) — re-render with Chrome print-to-PDF or Playwright after any capabilities copy change, and bump the "Rev." month in the header. `_tools/` is stripped from the Netlify build.
Business context recorded for whoever edits this next: **DSW is not ISO certified** (Chris's call, tied to whether he wants to scale). The "AWS" welding credential stays deliberately unspecific. Neither the site nor the PDF claims any certification.

### Session 11 — code cleanup (2026-09-03, **not yet committed**)

Goal: same site, less code. Verified by pixel-diffing full-page screenshots of all 7 pages at 1280 px and iPhone 13 width, before vs after (lazy-loading stripped in both so image timing couldn't fake a diff). Every diff is one of the three intentional changes below; everything else is identical.
1. **One stylesheet.** `tokens.css` is gone. It was 313 lines of which 22 tokens plus the `html`/`body` base rules and `.eyebrow` were actually in effect (its `:root` was mostly shadowed by a second `:root` in `style.css`). Those live at the top of `style.css` now; the two page-local `<style>` blocks in `about.html` and `gallery.html` (≈450 lines) moved into `style.css` under their own headers. 48 dead rules removed (`.promise-*`, `.mat-*`, `.ind-*`, `.credentials-*`, `.photo-grid`, `.section*`, …). All `.blue-light/.text-mid/.dark/.border-light` aliases collapsed onto their real tokens; every hex that matched a token became `var(--…)`. `#0d2b4e` is now `--ink`.
2. **57 of 60 inline `style=""` attributes → classes** (`.form-*`, `.quote-form`, `.page-hero-inner`, `.section-eyebrow--brand`, `.cta-buttons--spaced`, `.inline-link`, `.footer-brand img`). The three left are per-image `object-position` values on capabilities backgrounds and the laser tile — that's data, not styling. The `<h1 style=…>` on four pages duplicated `.page-hero h1` exactly. The mobile `.nav-logo img { height: 28px }` rule was deleted because the inline 36px had always overridden it — nothing moved.
3. **Intentional visual changes:** (a) orange CTA `#dd5932` → `--cta: #c2410c` (white text now 4.9:1, was 3.78:1 — the last P1 contrast item); hover `--cta-hover`. (b) footer social icons get a 44 px hit area (WCAG 2.5.8) — they shift ~3 px. (c) gallery lightbox image has an alt.
4. `style-work.css` moved to `work/style-work.css` (only the five quarantined case-study pages use it; `capabilities.html` stopped loading it in session 5).
5. **Stale docs deleted:** `AUDIT-FINDINGS.md`, `copy-report.md`, `design-report.md`, `REAL-PARTS-COPY-DECK.md`. Every item in the audit's "what I'd do" list is done or superseded (Work page retired, contrast fixed here). The only audit thread not re-measured: white tile labels over photos rely on the tile scrim gradient for contrast. The copy deck's reasoning (why 4 program cards, kingpin work excluded, shop-wide numbers only) is captured in §2 above.

### Session 12 — About page layout + analytics (2026-09-03, **not yet committed**)

1. **About intro panel rebuilt (Henry picked "option B").** The navy panel next to the hero photo had been designed for a heading and four paragraphs; with the session-4 cut it held three floating sentences in 580 px of empty blue. Now: "Who We Are" label, heading "Same ownership since 2004.", the three facts as one paragraph, and the **four stats (2004 / 230,000 / 80 / 11) as a 2×2 grid inside the panel**. The separate white stat band is gone — same numbers, same order, relocated. If Chris wants the strip back it's a small revert (the old `.about-stats` markup is in git at `dddd542`). Photo is now 4:3 at its natural height; the About `h1` lost its centered wrapper so it lines up with every other page.
2. **Google Analytics 4** — Henry's choice over Netlify Analytics (free, shareable login, tracks PDF downloads and tel: taps). **Waiting on the Measurement ID (`G-…`)** to add the tag to all 7 pages.

**Next, in order (agreed 2026-09-03):** ~~SEO basics~~ (done, session 6) (sitemap, robots, LocalBusiness JSON-LD, GBP check) → photo pipeline (resize to display size, lazy-load, video re-encode; originals archived, quality judged on screen before commit) → code cleanup (one CSS file, dead rules out, inline styles to classes, unused photos + stale report docs removed).

---

## 4. Gotchas — read before editing

- **`tokens.css` must load BEFORE `style.css`.** `style.css` declares its own `:root` that redefines several token names with *different* values (`--navy` is `#141c2e` there, `#0D2B4E` in tokens). Loading tokens first lets style.css win on conflicts while filling the 26 gaps. **Do not delete `style.css`'s `:root` block** without a full regression pass — much of the site depends on it.
- **`--rule` is a border shorthand** (`1px solid rgba(...)`). Use it as `border: var(--rule)`. For a colour, use `--dark-border` (light surfaces) or `--rule-color`.
- **Don't regex-replace `"` for inch marks across a whole HTML file** — it corrupts SVG attributes. Restrict to body copy. (This bit me; caught and repaired.)
- Line endings are **mixed, not uniformly CRLF** — the HTML files edited in session 2 are LF, `netlify.toml` and the untouched originals are CRLF. Match whatever a file already uses when you edit it; normalising a file wholesale turns a three-line change into a 600-line diff and buries the real edit.
- **`netlify.toml` now has a build command** (`rm -rf work`). It did not before. If a future change adds a real build step, that command has to be composed with it (`rm -rf work && <build>`), not replaced — silently dropping it re-publishes the placeholder pages.
- The homepage rail carousel assumes `VISIBLE = 4`. Adding a 5th card re-enables the arrows automatically; going below 4 needs no change.

---

## 5. Open work, in priority order

Sessions 2 and 3 were committed and pushed as `67ab6d0` (confirmed identical on GitHub 2026-09-03).

**Sessions 4–7 committed (`353e7b8`, `cd5a4da`, `9b2025c`, `0e4f60d`).** Uncommitted right now (session 8): 3 html files, 9 resized + 1 new mp4, 73 deletions under `photos/`, `PROJECT-LOG.md`. Session 5 was: `index, capabilities, about, gallery, contact, thank-you, 404, _redirects, PROJECT-LOG.md`, deleted `work.html`. Was: `index, capabilities, about, gallery, contact, PROJECT-LOG.md` and the two `work/` pages above. Also ~16 files that show as modified but are CRLF-only noise from the Windows checkout (`git diff --ignore-cr-at-eol --stat` is empty) — harmless to commit.

Suggested commit, run from the local clone in PowerShell:

```
git add -A
git commit -m "Photo pipeline: resize hero backgrounds, lazy-load, re-encode hero video, drop 72 unused photos"
git push
```

Netlify auto-builds from `main`. After the deploy, confirm three things on
`dswcutting.netlify.app`: submitting the contact form lands on `/thank-you`;
`/work/frame-rail-components.html` returns the 404 page; and the Quality and
Local Delivery homepage tiles jump to their sections.

### Before showing the boss
1. ~~Commit + push sessions 2–3~~ — done (`67ab6d0`). **Commit + push session 4** ← the only thing standing between the Chris-review edits and the staging URL
2. ~~Thank-you page~~ — done, session 3.
3. ~~The `[FILL: …]` placeholders~~ — quarantined, session 3. Still outstanding as *content*: the 15 placeholders need real, non-identifying values before those 5 pages can be published, and 4 of them ask for a customer identity that can never be published as-is. Rewrite those 4 as industry framing ("a trailer OEM"), not a name.
4. ~~Quality & Inspection / Local Delivery sections~~ — done, session 3. **Henry to confirm** the FARO arm CMM + laser scanner wording, which was carried over from the homepage tile rather than verified independently.

### Remaining audit items (P1/P2)
5. ~~Orange `.cap-cta` contrast~~ — fixed session 11. White tile labels over photos not re-measured (scrim-dependent).
6. ~~Heading order~~ — fixed session 3.
7. ~~`loading="lazy"`~~ — session 8.
8. ~~Tap targets~~ — social icons fixed session 11; mobile menu links already ≥40 px.
9. ~~Contact intro duplicate~~ — rewritten session 5.

### P3 — design system debt
Largely paid down in session 11 (one stylesheet, dead rules out, inline styles to classes, hex → tokens). Still true: ~27 one-off font sizes and a handful of rgba() literals in `style.css`. Cosmetic; touch only when editing those rules anyway.

### Domain cutover
10. **Confirm the payment method behind the Nov 5 renewal.** Only hard deadline in the project.
11. Get **Adam Witherspoon** (no longer with DSW) off the domain's technical contact record.
12. Cutover = **two DNS records**, in Wix, nameservers untouched:
    - A `@` : `50.116.39.119` → **`75.2.60.5`**
    - CNAME `www` : `dswcutting.com` → **`dswcutting.netlify.app`**
    - Set **`www.dswcutting.com` as the primary domain** in Netlify — all existing indexed URLs use www.
    - **Never click "Try Again"** on Wix's red "domain is set to point away from Wix" banner. That banner is correct; the button would overwrite the records.
    - Full zone backup: `DNS-SNAPSHOT-dswcutting.com.md`.
13. Post-cutover cleanup: remove `ip4:50.116.39.119` from the SPF record once the WordPress box is decommissioned; work out what sends via Amazon SES (3 DKIM keys); delete ~15 dead Yahoo CNAMEs.
14. **Chris McIlvaine's sign-off** before the domain moves.

---

## 6. Reference documents in this repo

| File | Contents |
|---|---|
| `DNS-SNAPSHOT-dswcutting.com.md` | Complete DNS zone as of 2026-08-18 — the rollback record |
| `_redirects` | Legacy URL map + retired-page redirects |
| `DSW-Capabilities-Sheet.pdf` / `_tools/capabilities-sheet.html` | Buyer-facing capabilities PDF and its source |
| `dsw-site-edit-spec.md` (Henry's copy, not in repo) | The Chris-review spec implemented in session 4 |
| `_originals/` (gitignored, Henry's PC only) | Camera originals, pre-resize web copies, unused photos |
