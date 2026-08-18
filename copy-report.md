I've read all 7 files completely and cross-checked every numeric claim, label, and CTA. Here is the full audit.

---

# DSW Cutting — Pre-Launch Copy & Content Consistency Audit

**Files audited:** `/root/dsw-website/index.html`, `capabilities.html`, `work.html`, `gallery.html`, `about.html`, `contact.html`, `404.html`
**No files were modified.**

A note on rendering, because it changes severity: `.section-eyebrow`, `.mono-eyebrow`, `.cap-tag`, `.cap-heading`, `.cap-spec-pill`, `.work-pill`, `.nav-cta`, `.cap-cta`, `.case-cta__btn`, `.work-industry-v2__cta` and all `page-hero h1` elements are forced to ALL CAPS by CSS. So source-level casing in those is invisible to customers. Everything flagged below as *user-visible* has been checked against that.

---

## 1. NUMBERS THAT DISAGREE

### Full inventory of every numeric claim

| Claim | Where |
|---|---|
| Founded 2004 | index 38, 63, 393, 437, 460; capabilities 282; work 183; gallery 377, 522, 542; about 6, 229, 243, 263, 287, 330; contact 84, 94; 404 61, 71 |
| 230,000 sq ft | index 42, 57, 417; gallery 373, 397; about 237, 244, 267; contact 84; 404 57 |
| 80 employees | gallery 402, 515; about 244, 272 |
| 12 fiber lasers | gallery 380, 406; about 276 |
| 8 fiber lasers | index 100 |
| 7 Mitsubishi fiber lasers | capabilities 45, 56; gallery 380 |
| 2×6 kW · 2×8 kW · 3×10 kW | capabilities 46, 56 |
| 1 Bescutter Giga 20 kW, 11.5 ft wide | capabilities 47, 48, 56; gallery 381 |
| 5 automation-fed lasers / 2 automation towers | capabilities 50, 56 |
| 2 plasma tables | index 100; capabilities 51, 56 |
| Max 2″ plate | index 69, 100; capabilities 53, 56 |
| 45° bevel / 0–45° | index 276; capabilities 49, 53, 56, 76 |
| 4 tube lasers | index 113; capabilities 70–73, 81; gallery 381–382 |
| 3 tube machines (North Birmingham) | about 245 |
| 40 ft outbound | index 76, 113, 271; capabilities 74, 81; work 122; about 244 |
| 30 ft infeed/outfeed (Apex) | capabilities 81 |
| 14″ max OD | index 113; capabilities 75, 81 |
| 11″ OD (Apex) | capabilities 81 |
| 5 Mitsubishi press brakes | capabilities 95, 105; gallery 460 |
| 1 robotic bending cell | capabilities 96, 105 |
| Robotic bending cell**s** (plural) | about 244, 305 |
| 250 tons max | index 126; capabilities 97, 105 |
| 160″ max length | index 126; capabilities 98, 105 |
| Davi roller: 80″ wide, 0.4375″ thick, 8″ min dia | capabilities 100–102, 105 |
| 2 Haas VF-3YT-50, 5-axis, 50-taper | capabilities 119–121, 126; gallery 383 |
| 4 weld robots (OTC Daihen) | index 139; capabilities 140–141, 148; gallery 384, 465 |
| 3 spot weld machines | capabilities 162, 168 |
| 16mm max tap | capabilities 165, 168; gallery 470 |
| 2 flatbed trucks + 4 Conestoga = 6 trucks | index 178, 423 |
| 2 shifts / near 24/7 | capabilities 56, 105; gallery 390, 410; about 244, 304, 305 |
| 1960s knife shop origin | about 242 |
| 2024 tube facility opened | about 245 |
| 1 business day response | work 173 |
| © 2026 | all footers |

### Findings

**1.1 — CRITICAL: "one roof" is contradicted by a second building.**
`about.html:245`: *"In 2024, we opened a dedicated tube-laser facility in North Birmingham — three machines under one roof, deepening our tube-cutting capacity…"*
This directly conflicts with `index.html:418` *"Single facility, one roof"*, `gallery.html:373` *"230,000 square feet"* framed as one floor, `gallery.html:380` *"Twelve fiber lasers under one roof"*, and `about.html:309–310` *"One Roof. One Point of Contact." / "Laser, tube laser, bending, welding, and finishing under one roof."* It also says **three** tube machines while every other page says **four** (`index.html:113`, `capabilities.html:81`, `gallery.html:381`). A customer cannot tell whether tube work happens in the 230,000 sq ft building or somewhere else, or where the fourth machine is.
**Fix:** Either delete `about.html:245` entirely, or rewrite to reconcile: *"In 2024 we added a dedicated tube-laser bay in North Birmingham. Four tube lasers now run there, all under DSW management and on the same schedule as the main floor."* And change `index.html:418` from `Single facility, one roof` to `Two Birmingham locations`. If tube is in fact in the main building, delete line 245 and keep "one roof" everywhere.

**1.2 — CRITICAL: laser count reads as 8 → 7 → 12 across three pages with no bridge.**
- `index.html:100`: `8 fiber lasers &nbsp;·&nbsp; 2 plasma tables &nbsp;·&nbsp; Up to 2″ plate`
- `capabilities.html:45`: `7× Mitsubishi Fiber Lasers`
- `capabilities.html:56`: `Seven Mitsubishi fiber lasers across three power tiers…`
- `gallery.html:406` / `about.html:276`: `12` / `Fiber Lasers`

**These do reconcile arithmetically** — 7 Mitsubishi flat (2+2+3 = 7 ✓) + 1 Bescutter Giga = **8 flat fiber lasers**; plus 4 tube fiber lasers = **12 total**. But nothing on the site says so. A buyer clicking index → capabilities sees the number *drop* from 8 to 7, which reads as an inflated homepage claim.
**Fix:** `index.html:100` → `8 flat fiber lasers &nbsp;·&nbsp; 2 plasma tables &nbsp;·&nbsp; Up to 2″ plate`. `capabilities.html:45` → `7× Mitsubishi Fiber Lasers + 1× Bescutter Giga`. `gallery.html:406` label → `Fiber Lasers (8 Flat + 4 Tube)` to match `about.html:276`.

**1.3 — "Eight flat cutters" undercounts flat-cutting machines.**
`gallery.html:380`: *"Twelve fiber lasers under one roof. Eight flat cutters — seven Mitsubishi plus a Bescutter Giga…"*
"Flat cutters" naturally includes the two Messer plasma tables, which also cut flat plate — making 10, not 8.
**Fix:** *"Eight flat-bed fiber lasers — seven Mitsubishi plus a Bescutter Giga super-large-frame with a 3D bevel head — alongside two Messer high-definition plasma tables."*

**1.4 — Robotic bending cell: one or several?**
`capabilities.html:96` `1× Robotic Bending Cell` and `capabilities.html:105` *"One press brake is paired with a robotic arm"* (singular) vs `about.html:244` *"robotic bending cells"* and `about.html:305` *"robotic bending cells maximize throughput"* (plural).
**Fix:** Make both singular on about: line 244 → `…a robotic bending cell, and tube laser systems…`; line 305 → `Automated material towers and a robotic bending cell maximize throughput across both shifts…`

**1.5 — Press brake pills read as 6 machines; the body says 5.**
`capabilities.html:95–96` stacks `5× Mitsubishi Press Brakes` and `1× Robotic Bending Cell` as separate pills, implying six machines. `capabilities.html:105` says the robotic one *is* one of the five.
**Fix:** Change pill 96 to `1 of 5 Robot-Tended` so the arithmetic is unambiguous.

**1.6 — Alt text names a press brake brand that contradicts the equipment list.**
`gallery.html:432`: `alt="FANUC R-2000iC robot arm — Diamond press brake cell"`. Capabilities says all five brakes are Mitsubishi; no FANUC or Diamond equipment appears anywhere else on the site.
**Fix:** `alt="Robotic bending cell — robot arm loading the press brake"` unless Diamond/FANUC is real, in which case add it to `capabilities.html:95–96`.

**1.7 — Alt text names a fifth tube-laser brand.**
`gallery.html:429`: `alt="Tube laser chuck — Dairuike, square tube loaded"`. "Dairuike" appears nowhere else; the site lists exactly four tube machines (BLM, HyTube, Titan, Apex).
**Fix:** `alt="Tube laser chuck gripping square tube"`.

**1.8 — "Two shifts" vs "nearly 24 hours a day."**
`gallery.html:390` *"We run two shifts, nearly 24 hours a day"* sits directly beside the stat `2` / `Shifts Daily` (`gallery.html:410`). Two shifts is ~16 hours. A manufacturing buyer will do that math instantly.
**Fix:** *"We run two shifts, with automation towers cycling sheet stock unattended overnight — the floor is rarely cold."* Same edit needed at `about.html:244` (*"running two shifts nearly 24/7"*) and `about.html:304–305`.

**1.9 — 2″ max plate is attributed to lasers on the homepage, plasma on capabilities.**
`index.html:69–70` presents `2″` / `Max Plate Thickness` as a facility-wide headline stat, and `index.html:100` bundles it into the laser tile. `capabilities.html:56` attributes it specifically to the plasma tables: *"Two Messer high-def plasma tables… handle carbon plate up to 2″."* No max laser thickness is ever stated.
**Fix:** Either state the laser max separately in `capabilities.html:53`, or change `index.html:70` label to `Max Plate Thickness (Plasma)`.

**1.10 — Facility stat sets disagree between pages.**
`index.html:57–76` = Square Feet / Founded / Max Plate Thickness / Max Tube Outbound. `gallery.html:397–411` = Square Feet / Employees / Fiber Lasers / Shifts Daily. `about.html:263–277` = Year Established / Square Feet / Employees / Fiber Lasers (Flat + Tube).
Three different four-stat sets, and `Founded` (index) vs `Year Established` (about) label the same 2004.
**Fix:** Standardise the label on `Founded`. Align gallery and about to the identical four stats.

**1.11 — "Founded 2004" vs "acquired the business in 2004."**
`index.html:38`, footer taglines, and `about.html:263` all say founded/established 2004. `about.html:242–243` says the business traces to a 1960s knife shop and that Chris McIlvaine *acquired* it in 2004.
**Fix:** Change the stat label at `about.html:264` to `Under Current Ownership` and add a clarifier at `index.html:38`: `Birmingham since the 1960s`. Or accept 2004 as the founding of DSW and soften `about.html:242` to *"DSW's building has been cutting metal in Birmingham since the 1960s…"*

**1.12 — "Newly added" is now stale.**
`capabilities.html:47` `Bescutter Giga 20 kW`, `capabilities.html:56` *"A newly added Bescutter Giga"*, `capabilities.html:81` *"a newly added Bescutter Apex 3030"*, `gallery.html:382` *"a newly added Bescutter Apex 3030"*, `capabilities.html:123` pill `Recently Installed`, `capabilities.html:126` *"as the cells come fully online"*, and `about.html:245` dates the addition to **2024**. It is now 2026.
**Fix:** Delete "newly added" from `capabilities.html:56, 81` and `gallery.html:382`. Replace pill `capabilities.html:123` with `50-Taper Rigidity`. Rewrite `capabilities.html:126` final sentence (see 5.2).

**1.13 — Address given with and without ZIP.**
`contact.html:72` `816 30th St N, Birmingham, AL 35203` vs `gallery.html:415` `816 30th St N · Birmingham, AL`.
**Fix:** Use the full ZIP form in both: `816 30th St N · Birmingham, AL 35203`.

**1.14 — Reconciles correctly, no action:** trucks (2 flatbed + 4 Conestoga = `6 Trucks`, index 178/423 ✓); weld robots (4 everywhere ✓); square footage (230,000 everywhere ✓); employees (80 everywhere ✓); tonnage, 160″, 40 ft, 14″ OD, 16mm tap, Haas count, spot-weld count (all ✓).

---

## 2. TERMINOLOGY DRIFT

**2.1 — The forming capability has four names.** Highest-visibility drift on the site.
- `index.html:123` `Press Brake &amp; Forming`
- `index.html:120` alt `Press Brake & Robotic Bending`
- `capabilities.html:93` `Press Brake &amp; Rolling`
- `gallery.html:459` `Press Brake & Roll`
**Fix:** Pick `Press Brake & Rolling` and use it in all four places.

**2.2 — The cutting capability has three names.**
`index.html:97` `Laser &amp; Plasma` · `capabilities.html:43` `Laser Cutting &amp; Plasma` · `gallery.html:454` `Laser & Plasma`.
**Fix:** `Laser Cutting & Plasma` everywhere.

**2.3 — First-piece / first-article / FAIR — four terms for one process.**
`index.html:255` pill `FAIR`; `index.html:253` *"First article through approved PPAP"*; `index.html:362` `First-Piece Verification`; `index.html:363` *"first-piece inspection"*; `capabilities.html:148` *"from first piece through"* (unhyphenated); `capabilities.html:228` `First-Piece Verification`; `about.html:289` *"first-piece sampling verification"*; `about.html:299` `First-Piece Sampling Program`; `gallery.html:387` *"first-article and in-process inspection"*; `gallery.html:475` *"First-article inspection"*; `work.html:92` *"first-piece verified"*.
**Fix:** Standardise on **First-Piece Verification** as the program name and **first-piece inspection** in body copy. Replace the `FAIR` pill at `index.html:255` with `First-Piece` (see 5.9). Fix `capabilities.html:148` to `first-piece`.

**2.4 — Inspection verification pills conflict inside the same carousel.**
`index.html:258` `FARO Verified` and `index.html:312` `CMM Verified` — the same FARO arm CMM, two labels, two cards apart. Also `index.html:411` `FARO Arm` vs `FARO arm CMM` at `index.html:165, 363`, `gallery.html:386, 475`, `work.html:107`.
**Fix:** Use `CMM Verified` in both pills; use `FARO arm CMM` in all prose and set `index.html:411` to `FARO Arm CMM`.

**2.5 — Automation described five ways.**
`capabilities.html:50` `2× Automation Towers`; `capabilities.html:56` *"five automation-fed"*; `index.html:152` *"laser tower auto-load · unattended runs"*; `gallery.html:386` *"Automation towers load, run, and unload sheet stock through the night"*; `gallery.html:455` *"Automation towers handle unmanned sheet cycling"*; `about.html:305` *"Automated material towers"*.
**Fix:** `Automation towers` as the noun everywhere; `unattended` (not "unmanned") as the adjective — `gallery.html:455` → *"Automation towers handle unattended sheet cycling for high-volume jobs."*

**2.6 — Plasma spec named three ways.** `capabilities.html:51` `2× Messer HD Plasma` · `capabilities.html:56` *"high-def plasma"* · `gallery.html:455` *"high-definition plasma"*.
**Fix:** `high-definition plasma` in prose, `HD Plasma` in pills only.

**2.7 — Uptime described four ways.** *"near-24/7"* (`capabilities.html:56`) · *"nearly 24 hours a day"* (`gallery.html:390`) · *"nearly 24/7"* (`about.html:244, 305`) · *"Near 24/7"* (`about.html:304`).
**Fix:** After the credibility fix in 1.8, use `near-24/7` consistently, hyphenated when attributive.

**2.8 — Who reviews the quote: three job titles.**
`index.html:352` *"Our sales engineers — people with real manufacturing backgrounds"*; `index.html:437` / `capabilities.html:232` / `about.html:288` *"Our sales team has real manufacturing experience"*; `about.html:295` *"Our sales engineers understand fabrication"*; `work.html:173` *"a sales engineer with manufacturing experience"*; `capabilities.html:232` *"Our engineering staff"*; `gallery.html:516` *"program managers"*.
**Fix:** Use **sales engineers** consistently for the quoting role; reserve **engineering team** for DFM/fixture work.

**2.9 — Tooling capability named four ways.**
`capabilities.html:180` `Jig &amp; Fixture Creation` · `capabilities.html:182` pill `In-House Fabrication` · `gallery.html:478` `07 — Jig & Fixture` with h3 `In-House Tooling` · `index.html:257` pill `In-House Tooling`.
**Fix:** `Jig & Fixture` as the section name, `In-House Tooling` as the pill/benefit label — and make `gallery.html:479` h3 read `Jig & Fixture` so the step number and title agree (every other ops step is parallel).

**2.10 — Outside finishing named three ways.**
`capabilities.html:201` `Partner Vendor Services` · `gallery.html:485` *"Partner vendor services available for powder coat, plating…"* · `index.html:311` pill `Finishing Managed`.
**Fix:** `Managed Finishing` as the section title on capabilities (clearer to a buyer than "Partner Vendor Services", which describes DSW's org chart, not the customer's benefit) and `Finishing Managed` in pills.

**2.11 — Industries section eyebrow differs.** `index.html:190` `Industries Served` vs `work.html:34` `Industries We Serve`.
**Fix:** `Industries We Serve` in both.

**2.12 — Page name vs nav label vs URL.** Nav says `The Shop` on all 7 pages; the file is `gallery.html`; `work.html:35` h1 is `The Work` but the nav label is `Work` and the title is `Work — DSW Cutting`.
**Fix:** Set `work.html:35` h1 to `Work` to match nav and title. Leave "The Shop" as-is but consider redirecting `/gallery` → `/shop` so the URL matches the label a customer clicked.

**2.13 — Company descriptor drifts.** `index.html:6` *"Precision Metal Fabrication"* · footer tagline (all 7 files) *"Precision Fabrication"* · `about.html:243` *"precision metal fabrication operations"*.
**Fix:** `Precision Metal Fabrication` everywhere.

**2.14 — Industry taxonomy contradicts itself in prose.** `index.html:437` says *"heavy equipment, agriculture, data centers, and power systems"* while the site's own seven industry names (`index.html:196–226`, `work.html:46–136`) are Heavy Equipment, Trucking & Trailer, Agricultural Equipment, Power & Data Infrastructure, Industrial Equipment, Construction & Infrastructure, Material Handling.
**Fix:** `index.html:437` → *"…with customers in heavy equipment, agricultural equipment, and power and data infrastructure."*

**2.15 — "Super Large Frame" hyphenation.** `capabilities.html:48` `Super Large Frame · 11.5 ft Wide` vs `capabilities.html:56` / `gallery.html:381` *"super-large-frame"*.
**Fix:** Use `Super-Large-Frame` in the pill.

---

## 3. CAPITALIZATION INCONSISTENCY

**3.1 — Section headings split Title Case vs sentence case by page, with no rule.** This is the most visible casing problem because `.cta-band h2`, gallery `h2`, `.work-visual-strip__title` and `.case-cta__heading` have no `text-transform`, so authored case renders verbatim.

*Title Case pages:* `index.html:86` `Built to Run. Built to Last.` · `191` `Built for the Demands of Heavy Industry.` · `238` `Real Parts. Real Programs.` · `336` `From First Contact to Final Part.` · `386` `What You Can Count On.` · `435` `Built on Partnership. Proven by Performance.` · `448` `Let's Build Something Together.` · `about.html:241` `A Shop Reborn in Steel City.` · `286` `We're a Product Partner. Not Just a Vendor.` · `318` `Ready to Talk?` · `404.html:39` `This One Didn't Make the Cut.` · `contact.html:38` `Let's Build Something Together.`

*Sentence case pages:* `gallery.html:373` `230,000 square feet built for production at scale.` · `426` `A look at how we work.` · `513` `The technology is only part of the story.` · `work.html:147` `Bent, formed, cut, stocked, and shipped.` · `172` `Have a part like one of these?`

**Fix:** The trade-plain voice suits sentence case better, but Title Case is the majority. Pick **Title Case** for all `h2` section headings and convert the five sentence-case ones: `gallery.html:373` → `230,000 Square Feet, Built for Production at Scale`; `gallery.html:426` → `A Look at How We Work`; `gallery.html:513` → `The Technology Is Only Part of the Story`; `work.html:147` → `Bent, Formed, Cut, Stocked, and Shipped`; `work.html:172` → `Have a Part Like One of These?`

**3.2 — `gallery.html:441` breaks its own page's pattern twice over.** `How We Run the Floor` is the only Title Case h2 on a page where every other h2 is sentence case, *and* the only h2 on the page with no trailing period.
**Fix:** Covered by 3.1 — after converting the page to Title Case, add nothing; this becomes the correct pattern rather than the exception.

**3.3 — Homepage spec pills mix capitalized and lowercase segments within the same line.** `.cap-tile-spec` has no `text-transform`, so this renders exactly as authored:
- `index.html:100` `8 fiber lasers · 2 plasma tables · **Up to** 2″ plate` — third segment capitalized, first two not
- `index.html:152` `**Robotic** press brake · laser tower auto-load · unattended runs` — first capitalized, rest not
- `index.html:113` `40 ft max outbound · 14″ max OD · 4 machines` — all lowercase
- `index.html:126` `250 ton max · 160″ length · robot cell` — all lowercase
- `index.html:178` `2 flatbed trucks · 4 Conestoga 18-wheelers · Birmingham metro & regional` — mixed via proper nouns
**Fix:** Sentence case each segment (capitalize the first word of every segment) for all seven tiles: `8 fiber lasers · 2 plasma tables · Up to 2″ plate` is already correct in segment 3; change segments 1–2 to `8 flat fiber lasers · 2 plasma tables · Up to 2″ plate`; `index.html:113` → `40 ft max length · 14″ max OD · 4 machines`; `index.html:126` → `250 tons max · 160″ max length · Robot cell`; `index.html:152` → `Robotic press brake · Laser tower auto-load · Unattended runs`; `index.html:165` → `FARO arm CMM · Laser scanner · PPAP capable`; `index.html:178` → `2 flatbed trucks · 4 Conestoga 18-wheelers · Birmingham metro & regional`.

**3.4 — Credential sub-labels mix Title Case and sentence case in one row.** `index.html:394–424`: `Birmingham, AL` / `Production Part Approval Process` / `Certified welding standards` / `CMM + laser scanner on-site` / `Single facility, one roof` / `Flatbed + Conestoga`.
**Fix:** Sentence case all six: `Production part approval process`, `Flatbed and Conestoga`.

**3.5 — `contact.html:32` `Get In Touch`** capitalizes the preposition. Standard title case lowercases prepositions under four letters; the site does this correctly elsewhere (`index.html:336` `From First Contact **to** Final Part`, `gallery.html:441` `How We Run **the** Floor`).
**Fix:** `Get in Touch`. Same issue at `index.html:385` `Why Customers Stay **With** Us` → `Why Customers Stay with Us`.

**3.6 — `index.html:107` alt text is a Title Case label while sibling alts are descriptive sentences.**
`alt="Tube Laser Cutting"` vs `index.html:94` `alt="Mitsubishi fiber laser cutting — radial spark burst"`, `159` `alt="FARO arm CMM precision inspection"`, `172` `alt="DSW delivery fleet — flatbed trucks outside facility"`. Also `index.html:120` `alt="Press Brake & Robotic Bending"` and `133` `alt="Robotic & Manual Welding"` and `146` `alt="Robotic Automation Cell"` are labels, not descriptions.
**Fix:** `index.html:107` → `alt="Tube laser cutting structural tube, sparks cascading"`; `120` → `alt="Operator unloading formed parts from a press brake"`; `133` → `alt="Robotic weld cell running a production assembly"`; `146` → `alt="Robotic arm loading a press brake in the bending cell"`.

**3.7 — Source-level only (invisible to users, fix for maintainability):** `index.html:42` types `230,000 SQ FT` in caps while its siblings on the same line type `Founded 2004` / `Birmingham, AL` in normal case — `.hero-eyebrow` uppercases all three anyway. Likewise `work.html:146` `Every day on the floor` is the site's only sentence-case eyebrow in source, but `.mono-eyebrow` renders it `EVERY DAY ON THE FLOOR`. Normalise both to Title Case in source for consistency with every other eyebrow.

---

## 4. PUNCTUATION INCONSISTENCY

**4.1 — Inch marks use two different characters between pages.** `index.html` uses the double-prime `″` (U+2033) at lines **69, 100, 113, 126**. `capabilities.html` uses the straight double quote `"` at lines **53, 56, 75, 81, 98, 100, 101, 102, 105**. These render noticeably differently — `2″` is upright and slanted, `2"` is a typewriter quote.
**Fix:** `″` is typographically correct for inches. Convert all of capabilities to `″`: `Up to 2″ Plate`, `Up to 14″ OD Round`, `Max 160″ Length`, `80″ Max Roll Width`, `0.4375″ Max Thickness`, `8″ Min Roll Diameter`, and the six instances in body copy at lines 56, 81, 105.

**4.2 — Trailing periods on headings are inconsistent, including on the h1s.**
`index.html:44` h1 `Your Long-Term<br>Fabrication Partner**.**` is the only h1 on the site with a terminal period — `capabilities.html:34`, `work.html:35`, `gallery.html:365`, `about.html:230`, `contact.html:33`, `404.html:34` all have none.
Among h2s: `gallery.html:441` `How We Run the Floor` (no period) is the only declarative h2 on the site without one.
**Fix:** Drop the period from `index.html:44` → `Your Long-Term<br>Fabrication Partner`. Add one to `gallery.html:441` → `How We Run the Floor.` Everything else is already consistent.

**4.3 — The `·` separator uses three different spacings, two of them inside the same footer block.**
- Footer tagline (all 7 files, e.g. `index.html:460`): `Precision Fabrication · Birmingham, AL · Founded 2004` — normal single spaces
- Footer bottom (all 7 files, e.g. `index.html:472`): `© 2026 DSW Cutting &nbsp;·&nbsp; Birmingham, Alabama` — non-breaking wide spacing
These two lines sit ~40px apart in the same footer and visibly disagree.
Elsewhere: `index.html:100` etc. use `&nbsp;·&nbsp;`; `capabilities.html:46, 49, 53` etc. use bare `·` with single spaces; `contact.html:84` uses `&nbsp;·&nbsp;`; `gallery.html:415` uses bare `·`.
**Fix:** Standardise on `&nbsp;·&nbsp;` everywhere. Change the footer tagline in all 7 files to `Precision Metal Fabrication&nbsp;·&nbsp;Birmingham, AL&nbsp;·&nbsp;Founded 2004`, and `capabilities.html` pills at 46, 48, 49, 53, 72, 76, 78, 143, 257, 258, 266, 268, 269 to match.

**4.4 — `·`, `&`, and `+` all used as the same "and also" connector.**
`index.html:412` `CMM **+** laser scanner on-site` and `index.html:424` `Flatbed **+** Conestoga` and `about.html:276` `Fiber Lasers (Flat **+** Tube)` use `+`, while everything comparable uses `·` or `&`.
**Fix:** `index.html:412` → `CMM and laser scanner on-site`; `index.html:424` → `Flatbed and Conestoga`; `about.html:276` → `Fiber Lasers (Flat & Tube)`.

**4.5 — Oxford comma applied in exactly one of seven parallel industry blurbs.**
`index.html:197` *"Structural brackets, weldments, **and** high-volume parts…"* — has it.
`index.html:202` *"Frame rails, cross members, structural tube assemblies."* · `207` *"Implement frames, heavy plate, high-volume formed components."* · `212` *"Enclosures, racks, precision-cut panels for critical systems."* · `217` *"Structural weldments, custom assemblies, complex geometry."* · `222` *"Structural steel, formed components, large-format plate work."* · `227` *"Conveyor systems, storage structures, fabricated frames."* — none have it.
Worse, the same lists on `work.html` **do** use "and": `work.html:122` *"Structural steel, formed components, **and** large-format plate work"*, `work.html:137` *"Conveyor systems, storage structures, **and** fabricated frames."*
**Fix:** Add `and` + Oxford comma to all six index blurbs so they match work.html: `202` → *"Frame rails, cross members, and structural tube assemblies."*; `207` → *"Implement frames, heavy plate, and high-volume formed components."*; `212` → *"Enclosures, racks, and precision-cut panels for critical systems."*; `217` → *"Structural weldments, custom assemblies, and complex geometry."*; `222` → *"Structural steel, formed components, and large-format plate work."*; `227` → *"Conveyor systems, storage structures, and fabricated frames."*

**4.6 — Arrow glyph appears on some "see more" links and not others.**
`index.html:88` `See Capabilities &rarr;` and `index.html:438` `See Capabilities &rarr;` and `index.html:240` `See All Work &rarr;` have arrows; `index.html:48` `See Capabilities` (the hero button) does not. `work.html:174` uses a literal `→` character rather than the `&rarr;` entity used on index (renders identically, but inconsistent in source).
**Fix:** Leave the hero button arrow-less (buttons vs text links is a defensible distinction), but make it explicit — and convert `work.html:174` to `&rarr;` for source consistency.

**4.7 — En dash used once on the entire site.** `capabilities.html:49` `0–45°` and `capabilities.html:56` `(0–45°)` are the only en dashes. Correct usage for a numeric range, so this is fine — but note that every other dash on the site is an em dash `—`, and there are no en dashes anywhere else because no other ranges exist. No action; flagged only so it isn't "corrected" to a hyphen later.

**4.8 — Em dash spacing is consistent (spaced em dash, `word — word`) across all 7 files.** Verified; no action.

**4.9 — Apostrophes are consistently straight (`'`) across all 7 files.** Zero curly apostrophes (`’`) anywhere. Consistent, so no visible defect — but a site that bothers with `″` primes and `—` em dashes reads as unpolished with typewriter apostrophes.
**Fix (optional, low):** Convert all 33 apostrophes to `’`. If you do this, do it in all 7 files at once — a half-conversion is worse than none.

**4.10 — Hyphenation drift.** `capabilities.html:148` *"from first piece through"* (unhyphenated) vs `first-piece` in 10 other places. `about.html:304` `Near 24/7` vs `near-24/7` attributive elsewhere. `about.html:245` `tube-laser facility` / `tube-cutting capacity` vs `tube laser` unhyphenated in all 8 other instances. `capabilities.html:48` `Super Large Frame` vs `super-large-frame`.
**Fix:** `capabilities.html:148` → `first-piece`; `about.html:304` → `Two-Shift, Near-24/7 Production`; `about.html:245` → `tube laser facility`.

**4.11 — Source-only:** `gallery.html` lines 441, 454, 459, 464, 469, 474, 478, 484, 501 use a raw `&` in text content where every other file uses `&amp;`. Renders identically; fix for consistency only.

---

## 5. TYPOS, GRAMMAR, AND AWKWARD PHRASING

**5.1 — Broken parallel construction, `about.html:244`.**
Current: *"What began as a small cutting operation is now a 230,000 sq ft facility employing 80 people, running two shifts nearly 24/7, robotic bending cells, and tube laser systems capable of 40-foot cuts — all a few miles from the Sloss Furnace smokestacks…"*
The list parses as "running [two shifts], [robotic bending cells], and [tube laser systems]" — you don't "run robotic bending cells nearly 24/7" as a parallel to running shifts, and the sentence is 48 words long.
**Fix:** *"What began as a small cutting operation is now a 230,000 sq ft facility with 80 employees, two shifts, a robotic bending cell, and tube laser systems that cut to 40 ft — a few miles from the Sloss Furnace smokestacks that put Birmingham on the map."*

**5.2 — Wrong voice, `capabilities.html:126`.**
Current: *"Full production specs will publish as the cells come fully online."*
"Specs will publish" needs the passive: specs are published, they don't publish themselves. Also "fully" appears twice in one sentence, and this is placeholder copy shipping at launch.
**Fix:** Delete the sentence, or replace with: *"Full production specs are available on request."*

**5.3 — Awkward and slightly false, `about.html:300`.**
Current: *"Your parts are to spec before we run quantity."*
Parts can't be to spec before they're made; "run quantity" is internal jargon.
**Fix:** *"We confirm the first piece is to spec before we run the full quantity."*

**5.4 — Faulty parallelism, `about.html:287`.**
Current: *"…producing the parts they need — accurately, affordably, and on time."*
Two adverbs joined to a prepositional phrase.
**Fix:** *"…producing the parts they need: on spec, on budget, and on time."*

**5.5 — Mixed metaphor, `gallery.html:377`.**
Current: *"Since 2004, we've expanded that footprint into one of the most capable fabrication floors in the region."*
You expand a footprint *to* a size; you don't expand it *into* a floor.
**Fix:** *"Since 2004 we've grown that footprint to 230,000 square feet, and the floor inside it to match."*

**5.6 — Self-contradicting sentence, `gallery.html:515–517`.**
Current: *"DSW has 80 people on the floor. Experienced operators who know the machines, program managers who know your account, and leadership that came up through fabrication — not through a spreadsheet."*
Program managers and leadership are not "on the floor," so the opening sentence contradicts its own list.
**Fix:** *"DSW has 80 people. Operators who know the machines, program managers who know your account, and leadership that came up through fabrication — not through a spreadsheet."*

**5.7 — Sentence fragment and off-taxonomy content, `work.html:62`.**
Current: *"From full-size farm and construction equipment down to retail and industrial mower lines."*
No verb. Also mentions *construction equipment* under the **Agricultural Equipment** heading, when the page has a separate Construction & Infrastructure section at line 121.
**Fix:** *"We build for everything from full-size farm equipment down to retail and industrial mower lines."*

**5.8 — Awkward, `work.html:47`.**
Current: *"Parts that ride hard and need to hold up to it."*
Parts don't ride hard — vehicles do; and "hold up to it" is vague.
**Fix:** *"Parts that take a beating on the road and have to hold up."*

**5.9 — Unexplained acronyms in customer-facing pills.**
`index.html:255` `FAIR` (First Article Inspection Report — reads as the English word "fair") and `index.html:311` `VMI` (Vendor Managed Inventory) appear with no expansion anywhere on the site. `index.html:405` credential value `AWS` with sub-label `Certified welding standards` is ambiguous — for the Power & Data Infrastructure audience the site explicitly targets, "AWS" reads as Amazon Web Services first.
**Fix:** `index.html:255` → `First-Piece`; `index.html:311` → `Managed Inventory`; `index.html:405–406` value → `AWS D1.1`, sub → `American Welding Society structural code`.

**5.10 — Vague credential wording, `index.html:406`.**
*"Certified welding standards"* is not idiomatic — standards aren't certified, welders and procedures are. As written it also over-claims without naming a certification body.
**Fix:** Covered in 5.9. If DSW has certified welders, say `AWS-certified welders`; if it welds *to* AWS code without certification, say `Welded to AWS D1.1`. Do not ship the current wording.

**5.11 — Jargon in a headline stat, `index.html:76` and `index.html:113` and `capabilities.html:74`.**
`Max Tube Outbound` / `40 ft max outbound` / `Up to 40 ft Outbound`. "Outbound" as a standalone noun is machine-operator vocabulary; a purchasing engineer scanning the stats bar reads it as shipping.
**Fix:** `index.html:76` label → `Max Tube Length`; `index.html:113` → `40 ft max length`; `capabilities.html:74` → `Up to 40 ft Part Length`. Keep "infeed/outfeed" at `capabilities.html:81` where the context is explicit.

**5.12 — Non-sequitur "or", `404.html:40`.**
Current: *"The page you're after has moved or no longer exists. Everything below is still right where it should be — or send us a drawing and we'll take it from there."*
The "or" joins two unrelated clauses.
**Fix:** *"The page you're after has moved or no longer exists. Everything below is still right where it should be. Or send us a drawing and we'll take it from there."*

**5.13 — Over-clever, `capabilities.html:189`.**
Current: *"We build to your drawings, and we build what holds your drawings in place."*
The wordplay obscures the point (they make the fixtures), and "what holds your drawings in place" is literally nonsense — fixtures hold parts, not drawings.
**Fix:** *"We build to your drawings — and we build the fixtures that hold your parts while we do it."*

**5.14 — Unit inconsistency.** `capabilities.html:165, 168` and `gallery.html:470` state tap capacity in **metric** (`16mm`) on a site that is otherwise entirely imperial (inches, feet, tons).
**Fix:** `Up to 16mm (5/8″) Tap Diameter` — or convert to `5/8″` and match the rest.

**5.15 — Odd decimal, `capabilities.html:101, 105`.** `0.4375" Max Thickness` — shops quote plate in fractions.
**Fix:** `7/16″ Max Thickness` (or `7/16″ (0.4375″)`).

**5.16 — Style drift in the same measurement.** `40 ft` (index 76, 113, 271; capabilities 74, 81; work 122) vs `40-foot` (`about.html:244`); `250 ton max` (`index.html:126`) vs `Max 250 Tons` (`capabilities.html:97`) vs `250 tons` (`capabilities.html:105`).
**Fix:** `40 ft` and `250 tons` everywhere; use `Max 250 tons` in pills.

**5.17 — Phone number format, `contact.html:80`.** `205.322.2021` — dot-separated is the least common US convention and hardest to scan.
**Fix:** `(205) 322-2021`.

**5.18 — Form label is a sentence where its siblings are nouns, `contact.html:63`.**
`Tell us about your project` sits alongside `Name *`, `Company`, `Email *`, `Phone` — and `.contact` label styling applies `text-transform: uppercase` with `letter-spacing: 0.2em`, rendering a long, hard-to-read all-caps sentence.
**Fix:** `Project Details`.

---

## 6. DUPLICATED COPY

**6.1 — The entire CTA band is duplicated verbatim between the homepage and the contact page.**
`index.html:448–449` and `contact.html:38–39` are character-identical:
> `Let's Build Something Together.` / `We're committed to being a long-term partner in your supply chain — not just a transaction. Reach out and let's talk about what you're building.`
On contact.html this is worse than redundant: it sits directly under the hero `Let's Talk` (line 33), so a visitor sees two "let's" headings stacked with no new information before reaching the form.
**Not justified.** **Fix:** Replace `contact.html:38–39` with form-specific copy: heading `Send Us Your Drawings`, body *"Tell us what you're building and a sales engineer will get back to you within one business day. Prints, part lists, or a rough sketch — all fine."*

**6.2 — A second CTA band is duplicated verbatim between gallery and about.**
`gallery.html:530–531` and `about.html:318–319` are identical: `Ready to Talk?` / `Send us your drawings and let's build something together.`
Combined with 6.1, the site has two competing CTA bands (`Let's Build Something Together.` and `Ready to Talk?`) that say the same thing in different words.
**Partially justified** (a repeated CTA at page end is normal) **but the wording should not differ.** **Fix:** Standardise all end-of-page CTA bands on one heading — `Ready to Talk?` — and vary only the sub-line by page: gallery → *"Come see the floor, or send drawings and we'll quote it."*; about → *"Send us your drawings and let's talk about what you're building."*

**6.3 — "Our sales team has real manufacturing experience" appears three times near-verbatim.**
`index.html:437` *"Our sales team has manufacturing experience — giving you real guidance on materials, process selection, and DFM at the quoting stage, not just a price."*
`capabilities.html:232` *"Our sales team has real manufacturing experience. When you call with a drawing, you're talking to someone who understands what it takes to make the part — and can suggest improvements before the order is placed."*
`about.html:288` *"Our sales team has real manufacturing experience. When you call DSW, you're talking to someone who understands your drawing, your material, your tolerances — and can tell you if there's a better way to make it before the quote goes out."*
`capabilities.html:232` and `about.html:288` share the same sentence, the same second-sentence structure, and the same "before the order/quote" close.
**Justified once, not three times.** **Fix:** Keep `about.html:288` as the full version. Cut `capabilities.html:232` to: *"Our engineering staff supports customers at every stage — DFM review, material selection, process optimization. Send a drawing and you'll get feedback, not just a number."* Cut `index.html:437`'s final clause.

**6.4 — The first-piece promise is stated four times.**
`index.html:363`, `about.html:289`, `about.html:300`, `work.html:92`, plus pills at `index.html:362` and `capabilities.html:228` and step `gallery.html:475`. Within about.html alone it appears twice in adjacent columns (line 289 body, line 300 differentiator) saying the same thing.
**Fix:** Delete the redundancy inside about.html — cut `about.html:289` entirely and let differentiator 02 (line 299–300) carry it.

**6.5 — The 40 ft tube claim is written three ways in three places.**
`index.html:271` *"I-beam, channel, and structural tube cut weld-ready to 40 ft — coped and beveled off the machine."*
`work.html:122` *"I-beam, channel, and structural tube cut weld-ready to 40 ft — coped, beveled, and ready to assemble off the machine."*
`capabilities.html:81` *"…weld-prep geometries come off the machine ready to assemble."*
The first two are the same sentence with a different ending, and the homepage card links directly to the work.html section that repeats it.
**Justified as a summary→detail pair, but they should match.** **Fix:** Make `index.html:271` the short version — *"I-beam, channel, and structural tube cut weld-ready to 40 ft."* — and let work.html carry the full sentence.

**6.6 — The industry blurbs are duplicated between index and work with only comma changes.**
`index.html:222` vs `work.html:122` (first clause), `index.html:227` vs `work.html:137`. See 4.5.
**Justified** (teaser → full section) **but the shared clause should be identical**, not differ only by an Oxford comma.

**6.7 — Three near-identical "one X, one Y, one Z" triads.**
`capabilities.html:211` *"One PO. One point of contact. One schedule."* · `index.html:307` *"One supplier, one schedule, one part number."* · `about.html:309` *"One Roof. One Point of Contact."*
Formulaic on its own; three in a row across pages reads as a tic.
**Fix:** Keep `capabilities.html:211`. Change `index.html:307` to *"Cut, formed, welded, machined, and finished before it ships — under one PO."* Keep `about.html:309` as a heading (different register, acceptable).

**6.8 — "One of the most capable [x] in the region" twice.**
`gallery.html:377` *"one of the most capable fabrication floors in the region"* · `about.html:243` *"one of the region's most capable precision metal fabrication operations."*
**Fix:** Keep the about.html version (it's part of the origin narrative); rewrite gallery per 5.5.

**6.9 — Same photo, four different alt texts** (`warehouse-cranes-stock.jpg`): `index.html:284` *"DSW material warehouse with overhead cranes and staged inventory"* · `index.html:441` *"DSW facility — overhead cranes and material inventory"* · `capabilities.html:240` *"DSW material warehouse with overhead cranes and structural steel inventory"* · `about.html:237` *"DSW facility — 230,000 sq ft Birmingham"*.
Same again for `finished-parts-pallets.jpg` (`index.html:302`, `work.html:87`, `capabilities.html:197`) and `hss-channel-stack.jpg` (`index.html:266` *"staged for tube laser cutting"* vs `work.html:117` *"in the DSW material yard"* vs `work.html:162` *"Structural steel channel stock"* vs `gallery.html:502` *"geometric grid"*) and `forklift-warehouse.jpg` (`work.html:132`, `gallery.html:498`).
The `hss-channel-stack.jpg` pair actively contradicts: one says the material is staged for cutting, the other says it's in the yard.
**Fix:** One canonical alt per image file, reused: `warehouse-cranes-stock.jpg` → `"DSW material warehouse — overhead cranes and structural steel inventory"`; `finished-parts-pallets.jpg` → `"Finished fabricated parts palletized and ready to ship"`; `hss-channel-stack.jpg` → `"Structural channel and HSS stacked in the DSW material yard"`; `forklift-warehouse.jpg` → `"Forklift moving staged material through the DSW warehouse"`.

**6.10 — Same photo described as two different processes.** `cap-coming-soon.jpg` is `alt="CNC machined parts"` at `capabilities.html:113` and `alt="Formed metal parts fanned in production"` at `work.html:150`. The filename indicates it is a placeholder that was never replaced.
**Fix:** Replace the image on both pages before launch; if that's not possible, use `alt="CNC machined parts"` on capabilities and pick a different photo for `work.html:150`.

---

## 7. TONE DRIFT

Target voice: professional, clean, approachable, direct, plain-spoken trade language.

**7.1 — Unverifiable regional superlative, `capabilities.html:81`.**
*"…give us the deepest tube capability in the Southeast."*
This is the single strongest marketing claim on the site and the least supportable. A competitor or a skeptical buyer will discount everything around it.
**Fix:** Replace with the fact that earns the claim: *"…make tube one of the deepest capabilities on our floor — four dedicated systems running round, square, and structural stock."*

**7.2 — Competitor disparagement, `about.html:288`.**
*"That's DFM guidance most shops don't offer."*
Unverifiable, and punching at unnamed competitors is off-register for a plain-spoken trade voice.
**Fix:** Delete the sentence. The preceding sentence already makes the point.

**7.3 — Hype cluster, `about.html:243–244`.**
*"…saw something others didn't"* / *"Two decades on, that bet has paid off."*
Founder-mythology register; "bet" is never set up, and "paid off" is self-congratulation aimed at the wrong audience (buyers care about capacity, not the owner's return).
**Fix:** *"In 2004, Chris McIlvaine bought the business and started reinvesting — in people, in technology, and in floor space. He hasn't stopped."* / *"Twenty-two years later, what began as a small cutting operation is a 230,000 sq ft facility…"*

**7.4 — Slogan register, `about.html:255`.**
*"Different tools. Same city. Same grit."*
Three-beat ad copy. "Grit" is the kind of word the rest of the site earns by describing machines instead.
**Fix:** *"Different tools. Same city. Same work."* — or cut the sentence and end on the previous one.

**7.5 — Undercutting hedge, `about.html:254`.**
*"Steel has always run through this city. We're **just** keeping the tradition alive."*
"Just" makes a 230,000 sq ft operation sound like a hobby.
**Fix:** *"Steel has always run through this city. We're keeping it running."*

**7.6 — Negative framing introduces a problem the buyer wasn't thinking about, `capabilities.html:211`.**
*"No handoffs. No finger-pointing."*
"Finger-pointing" plants the idea of supplier disputes. It also contradicts `about.html:310` *"Fewer handoffs"* — and "No handoffs" is literally untrue on a page titled *Partner Vendor Services*, which describes handing parts to outside finishers.
**Fix:** *"Fewer handoffs, one accountable supplier. Parts arrive finished and ready to use."*

**7.7 — Slogan in the hero, `index.html:45`.**
*"…for industry's most demanding production programs. We build relationships, not just parts."*
"Industry's most demanding" is a floating superlative with no referent. "We build relationships, not just parts" is the third partnership statement on the same page (see also lines 436 and 449).
**Fix:** *"Laser cutting, tube laser, press brake forming, and robotic welding for demanding OEM production programs. One floor, one point of contact."*

**7.8 — Same partnership claim three times on the homepage.** `index.html:45`, `436`, `449`. By the third the reader has learned nothing new.
**Fix:** Keep `436` (the partner section, where it's the point). Cut the claim from `45` per 7.7 and tighten `449` to *"Send drawings or a part list and we'll come back with real numbers."*

**7.9 — "Product Partner" is the wrong noun, `about.html:286`.**
*"We're a Product Partner. Not Just a Vendor."*
DSW doesn't make products, it fabricates parts to customer prints. "Product partner" is consultant-speak and conflicts with the site's own framing at `index.html:436` (*"an extension of your manufacturing operation"*).
**Fix:** `We're a Fabrication Partner.<br>Not Just a Vendor.`

**7.10 — Pun risk on the error page, `404.html:39`.**
*"This One Didn't Make the Cut."*
For a metal-cutting company, "didn't make the cut" is a joke about failed quality. It's charming to a marketer and slightly off to a quality engineer.
**Fix (optional):** `Nothing Here to Cut.` — keeps the trade wink without implying rejected parts.

**7.11 — Register is consistent and on-target elsewhere.** `work.html:107` *"Built to print."*, `capabilities.html:168` *"Parts leave DSW complete — ready to assemble."*, `gallery.html:444` *"No shortcuts, no exceptions."*, `gallery.html:480` *"Faster setup, tighter repeatability, no outsourcing wait."* — these are the voice the rest of the site should be edited toward.

---

## 8. PAGE TITLES AND META DESCRIPTIONS

**8.1 — Title format is inconsistent across five different patterns.**

| File | Line | Current title | Pattern |
|---|---|---|---|
| index.html | 6 | `DSW Cutting — Precision Metal Fabrication · Birmingham, AL` | Brand — Descriptor · Location |
| capabilities.html | 6 | `Capabilities & Materials — DSW Cutting` | Page — Brand |
| work.html | 6 | `Work — DSW Cutting` | Page — Brand |
| gallery.html | 6 | `The Shop — DSW Cutting` | Page — Brand |
| about.html | 6 | `About DSW Cutting — Birmingham's Fabrication Partner Since 2004` | Page+Brand — Tagline |
| contact.html | 6 | `Contact — DSW Cutting` | Page — Brand |
| 404.html | 6 | `Page Not Found — DSW Cutting` | Page — Brand |

Five conform to `Page — DSW Cutting`; index and about break it. The about title also double-mentions the brand.
**Fix:** Keep index as-is (a homepage legitimately leads with the brand), but bring about into line: `About — DSW Cutting`. If you want the SEO keywords, use `About — DSW Cutting | Birmingham Metal Fabrication Since 2004` and apply the same `| descriptor` suffix pattern to capabilities and work, or to none.

**8.2 — Six of seven pages have no meta description.**
Only `work.html:7` has one. `index.html`, `capabilities.html`, `gallery.html`, `about.html`, `contact.html`, `404.html` have none — meaning Google will scrape arbitrary body text for the homepage snippet.
**Fix:** Add to each (155–160 chars, matching work.html's plain-descriptive register):
- `index.html` — *"Precision metal fabrication in Birmingham, AL. Laser and tube laser cutting, press brake forming, robotic welding, and CNC machining under one roof since 2004."*
- `capabilities.html` — *"Fiber and tube laser cutting to 40 ft, plasma to 2″, press brakes to 250 tons, robotic welding, CNC machining, and in-house fixtures. Full equipment list and materials."*
- `gallery.html` — *"Inside DSW's 230,000 sq ft Birmingham fabrication floor — machines, process, and the team that runs two shifts a day."*
- `about.html` — *"DSW Cutting has fabricated metal in Birmingham, Alabama since 2004. 230,000 sq ft, 80 people, and a sales team that came up through manufacturing."*
- `contact.html` — *"Send drawings or a part list to DSW Cutting in Birmingham, AL. A sales engineer with manufacturing experience responds within one business day."*
- `404.html` — none needed (`noindex` is correctly set at line 7).

**8.3 — `work.html:7` meta description omits two of the seven industries it describes.**
Current: *"Trucking, agricultural, power and data infrastructure, heavy equipment, and industrial fabrication programs…"* — missing Construction & Infrastructure and Material Handling, both of which are full sections on the page.
**Fix:** *"Representative work by industry: trucking and trailer, agricultural, power and data infrastructure, heavy equipment, industrial, construction, and material handling — from our Birmingham, AL floor."*

**8.4 — Low priority, structural:** `capabilities.html` uses `<h3 class="cap-heading">` for its nine top-level section headings (lines 43, 68, 93, 117, 138, 160, 180, 201, 223, 244) where every other page uses `<h2>`. This affects how search engines and screen readers outline the page.
**Fix:** Promote them to `<h2 class="cap-heading">`.

---

## 9. CTA TEXT

### Complete inventory (rendered case shown; ALL CAPS where CSS forces it)

| Label | Rendered | Locations |
|---|---|---|
| `Contact Us` | ALL CAPS (nav) | index 23, capabilities 24, gallery 355, about 219, contact 23, 404 24 |
| `Get a Quote` | ALL CAPS (nav) | **work 25** |
| `Contact Us` | Title Case (btn-primary) | index 47, index 451, gallery 533, about 321 |
| `Get a Quote` | Title Case (btn-primary) | **404 43** |
| `Contact Us` | ALL CAPS (cap-cta) | capabilities 58, 83, 107, 128, 150, 170, 191, 213, 234, 273 |
| `Contact Us` | ALL CAPS (industry cta) | work 48, 63, 78, 93, 108, 123, 138 |
| `Start a Conversation →` | ALL CAPS | work 174 |
| `Send Message` | Title Case | contact 66 |
| `See Capabilities` | Title Case (btn-outline) | index 48 |
| `See Capabilities →` | Title Case (link) | index 88, 438 |
| `See All Work →` | Title Case (link) | index 240 |
| `Get a Quote` | Title Case (timeline step title) | index 351 |
| `Contact` | footer link | all 7 files |

### Findings

**9.1 — CRITICAL: the persistent nav CTA changes label on one page.**
Six pages show `CONTACT US` in the nav; `work.html:25` shows `GET A QUOTE`. This is the single most-seen element on the site, and it flips as the user navigates.
**Fix:** `work.html:25` → `Contact Us`.

**9.2 — The primary button changes label on the 404 page.**
Every other `.btn-primary` on the site says `Contact Us`; `404.html:43` says `Get a Quote`. Combined with 9.1, `Get a Quote` appears in exactly the two places where it can only look like an oversight.
**Fix:** `404.html:43` → `Contact Us`. (Or, if "Get a Quote" is the intended primary CTA sitewide, change all 14 `Contact Us` buttons instead — but pick one and apply it everywhere.)

**9.3 — Four different verbs for the same destination.**
`Contact Us` / `Get a Quote` / `Start a Conversation →` / `Send Message` all lead to the same form. `work.html:174` `Start a Conversation →` is the only instance of its wording on the site and reads softer than everything around it.
**Fix:** `work.html:174` → `Contact Us &rarr;`, or promote one verb sitewide. Recommended set: **`Get a Quote`** for primary conversion buttons, **`Contact Us`** for the nav, **`Send Message`** for the form submit only. If you adopt that, change `index.html:47, 451`, `gallery.html:533`, `about.html:321`, `404.html:43` to `Get a Quote`, and all 17 `.cap-cta` / `.work-industry-v2__cta` instances to `Get a Quote` as well.

**9.4 — `Get a Quote` is used as both a CTA label and a process step name.**
`index.html:351` is the timeline step `01 — Get a Quote`, describing what happens after you contact them. Two lines of the same page also use `Get a Quote` as a button elsewhere on the site. A step title should describe the step, not command the user.
**Fix:** `index.html:351` → `Send Drawings` (which is exactly what the step body at line 352 describes).

**9.5 — Capabilities repeats an identical button eleven times on one page.**
`capabilities.html` lines 24, 58, 83, 107, 128, 150, 170, 191, 213, 234, 273 all read `CONTACT US`. Eleven identical buttons stacked down a scroll reads as a template artifact rather than an invitation.
**Fix:** Keep the nav CTA and one CTA at the page end. Delete the per-section buttons at 83, 107, 128, 150, 170, 191, 213, 234 and keep 58 and 273. Alternatively, make them contextual: `Quote a Laser Job`, `Quote a Tube Job`, `Quote a Forming Job` — which is stronger, but only if you'll maintain them.

**9.6 — The contact page's nav CTA links to the page the user is already on.** `contact.html:23` renders `CONTACT US` linking to `contact.html`.
**Fix:** Suppress or disable the nav CTA on contact.html, or change it to `Call 205.322.2021` with a `tel:` link.

**9.7 — The form submit doesn't match the form's purpose.** `contact.html:66` button says `Send Message` while the Netlify form is named `quote-request` (line 42) and the section heading promises partnership.
**Fix:** `Send Drawings` or `Request a Quote` — matching whichever verb you standardise on in 9.3.

**9.8 — Nav CTA and footer link disagree on the same destination.** Nav says `Contact Us`; the footer link on all 7 pages says `Contact`.
**Fix:** Acceptable as-is (nav CTAs are conventionally verb phrases, footer links are nouns) — but note it if you want strict parity.

**9.9 — The one-business-day promise appears on exactly one page.**
`work.html:173` *"…a sales engineer with manufacturing experience will be in touch within one business day."* This is the strongest conversion line on the site and it doesn't appear on the contact page, the homepage CTA, or the 404.
**Fix:** Add to `contact.html` (per 6.1) and to `index.html:449`.

**9.10 — Two homepage tiles promise capabilities the target page doesn't have.**
`index.html:158` `Quality & Inspection` links to `capabilities.html` with no anchor — and capabilities has **no** inspection section at all (only a `First-Piece Verification` pill inside Engineering). `index.html:171` `Local Delivery` (2 flatbeds, 4 Conestogas) links to `contact.html`, and logistics is never mentioned again anywhere on the site.
**Fix:** Add a `Quality & Inspection` section to capabilities.html (FARO arm CMM, laser scanner, first-piece, PPAP) with `id="quality"`, and a `Logistics & Delivery` section with `id="delivery"`, then point the tiles at those anchors. All other tile anchors (`#laser`, `#tube`, `#forming`, `#welding`) and all four work-card anchors (`#heavy-equipment`, `#construction`, `#agriculture`, `#industrial`) resolve correctly — these two are the only broken promises.

**9.11 — The homepage industries grid and work.html list the same seven industries in a different order.**
index leads with Heavy Equipment (line 196); work.html leads with Trucking & Trailer (line 46). Same seven, different priority signal.
**Fix:** Match the order. Use the work.html order on both, or the index order on both.

---

# TOP 10 FIXES

1. **`about.html:245` — resolve the "one roof" contradiction.** A second North Birmingham building with "three machines" contradicts `index.html:418` *"Single facility, one roof"*, `gallery.html:380` *"Twelve fiber lasers under one roof"*, `about.html:309–310`, and the four-tube-laser count on three other pages. Either delete the line or rewrite it and change `index.html:418` to `Two Birmingham locations`.

2. **`index.html:100` — stop the laser count from shrinking.** `8 fiber lasers` → `8 flat fiber lasers`, and `capabilities.html:45` `7× Mitsubishi Fiber Lasers` → `7× Mitsubishi Fiber Lasers + 1× Bescutter Giga`. The numbers reconcile (8 flat + 4 tube = 12) but nothing tells the reader that, so index → capabilities currently reads as a walk-back.

3. **`work.html:25` — fix the nav CTA.** `Get a Quote` → `Contact Us`. The site's most-repeated element changes label on exactly one page. Fix `404.html:43` the same way.

4. **`contact.html:38–39` — replace the copy duplicated verbatim from `index.html:448–449`.** On the contact page it stacks two "let's" headings and delays the form with a pitch the visitor already accepted. Replace with `Send Us Your Drawings` + the one-business-day promise from `work.html:173`.

5. **`about.html:244` — fix the broken parallel.** *"running two shifts nearly 24/7, robotic bending cells, and tube laser systems"* is a 48-word sentence with a faulty list. Rewrite per 5.1. Same edit fixes the singular/plural robotic-cell conflict with `capabilities.html:96`.

6. **Unify the forming capability name across four pages.** `index.html:123` `Press Brake & Forming`, `index.html:120` alt `Press Brake & Robotic Bending`, `capabilities.html:93` `Press Brake & Rolling`, `gallery.html:459` `Press Brake & Roll` → all `Press Brake & Rolling`.

7. **Convert `capabilities.html` inch marks from `"` to `″`** (lines 53, 56, 75, 81, 98, 100, 101, 102, 105) to match `index.html:69, 100, 113, 126`. Currently the same measurement is typeset two visibly different ways between the two pages a buyer compares most.

8. **`capabilities.html:81` — cut *"the deepest tube capability in the Southeast."*** The site's biggest unsupportable claim, sitting in its strongest capability section. Replace with the four-machine fact that actually earns it.

9. **Add meta descriptions to the six pages missing them** (`index`, `capabilities`, `gallery`, `about`, `contact`, `404`) and normalise `about.html:6`'s title to the `Page — DSW Cutting` pattern the other five follow. Right now Google writes the homepage snippet itself.

10. **Fix the two homepage tiles that link to content that doesn't exist** — `index.html:158` `Quality & Inspection` (no such section on capabilities) and `index.html:171` `Local Delivery` (mentioned nowhere but the homepage). Add both sections to `capabilities.html` with anchors, or remove the tiles. Bundle with `index.html:255` `FAIR` → `First-Piece` and `index.html:311` `VMI` → `Managed Inventory`, since unexplained acronyms sit in the same grid.
agentId: a5751b9cbbed1ca5e (use SendMessage with to: 'a5751b9cbbed1ca5e', summary: '<5-10 word recap>' to continue this agent)
<usage>subagent_tokens: 141499
tool_uses: 17
duration_ms: 600709</usage>