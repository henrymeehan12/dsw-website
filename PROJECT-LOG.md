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

**Uncommitted right now (session 4):** `index, capabilities, about, gallery, contact, PROJECT-LOG.md` and the two `work/` pages above. Also ~16 files that show as modified but are CRLF-only noise from the Windows checkout (`git diff --ignore-cr-at-eol --stat` is empty) — harmless to commit.

Suggested commit, run from the local clone in PowerShell:

```
git add -A
git commit -m "Chris review pass: strip machine branding, 3 towers / 9 weld windows, About cut, de-flowering"
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
5. Contrast still failing on **capabilities (13)**, **work (10)**, **gallery (1)** — mostly white text over photos and the orange `.cap-cta` (3.78:1). index is clean.
6. Heading order skips h1→h3 on `capabilities.html`.
7. No `loading="lazy"` on most images (61 site-wide).
8. Mobile tap targets under 44px (nav links 15px tall, social icons 20×23).
9. `contact.html` intro duplicates `index.html` verbatim.

### P3 — design system debt (see `design-report.md`)
51 font sizes, 73 colour literals, 5 near-identical navies, 52 inline `style=""` attributes, ~960 lines of dead CSS (41%). Invisible to customers; makes every future edit slower.

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
| `AUDIT-FINDINGS.md` | Prioritised audit — P0/P1/P2/P3 with file:line refs |
| `copy-report.md` | Full copy audit, 9 categories, every finding + recommended replacement |
| `design-report.md` | Full design-system audit, 8 categories + quantified drift table |
| `DNS-SNAPSHOT-dswcutting.com.md` | Complete DNS zone as of 2026-08-18 — the rollback record |
| `REAL-PARTS-COPY-DECK.md` | Card copy deck and the reasoning behind the 4-card rail |
| `_redirects` | Legacy URL map |
