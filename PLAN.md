# DSW Cutting — Website Revamp Plan
*Drafted: 2026-02-24 by Vector*

---

## Current Site Audit (dswcutting.com) — CORRECTED 2026-02-24

**Pages:**
- `/` — Homepage (generic intro, 3-step process, promise bullets)
- `/services-and-equipment/` — Full capabilities page (**has real content**)
- `/about-us/` — Company background (185,000 sq ft, Birmingham, since 2004)
- `/get-a-quote/` — Quote form
- `/contact/` — Contact
- `/careers/` — Careers

**What's on the Services & Equipment page (actual content):**
- **Laser Cutting & Plasma**: Mitsubishi Fiber Laser, tower-fed automation, light gauge to **1" plate**, Carbon/Stainless/Aluminum; Plasma cutting Carbon plate up to **2" thick** with bevel capability
- **Tube Laser**: BLM equipment, 2D/3D bevel, round+square+structural beams+C-channels+angles, up to **12.7" diameter**, up to **27' long**, BLM AR-Tube design software
- **Bending**: Mitsubishi/Murata electric + electric/hydraulic hybrid, robotic bending cell, up to **330 lbs** and **14ft long**
- **Quality Control**: first-piece sampling program
- **Welding**: manual + robotic weld cells + in-house fixture design
- **Partner Vendor Services**: plating, galvanizing (electro + hot-dip), e-coat, powder coating
- **Engineering**: DFM support, material selection, process selection
- **Materials**: Carbon Steel to 2" (A36, A572-50, A514, AR400), Stainless + Aluminum to 1"; mill direct pricing, JIT programs

**About Us highlights:**
- Since 2004
- 185,000 sq ft facility, Birmingham AL industrial district
- First-piece sampling QA program

**What's genuinely missing or weak:**
- Homepage doesn't surface any of the capability depth — it's just vague promises
- No shop photos on any page (20 Drive photos not used at all)
- No industries page — trucking/ag/data center customers never mentioned
- Services page has machine categories but no wattage, no bed sizes, no tolerances
- No part examples or portfolio
- All CTAs go to `/get-a-quote/` which feels transactional, not informational

**Verdict:** Capabilities are real and impressive — they're just buried on a page that nobody finds because the homepage doesn't direct them there. Also missing photos entirely. Revamp strategy: surface the specs, add the photos, replace vague homepage with something that shows capability at a glance.

---

## Proposed Site Structure

### 1. Hero (Full-bleed)
- **Image:** DSC01692.JPG — low-angle laser table shot, cinematic
- **Headline:** "Precision Metal Fabrication. Ready When You Are."
- **Subhead:** "Sheet metal, tube laser, and structural fabrication for industry's most demanding applications."
- **CTA:** "Get a Quote" + "See Our Work"

### 2. Capabilities (cards — now populated with real specs)
- **Laser Cutting** — Mitsubishi Fiber Laser, tower-fed automation; light gauge to 1" plate; Carbon / Stainless / Aluminum; Plasma up to 2" Carbon with bevel
- **Tube Laser** — BLM equipment (top-tier in Southeast); round + square + structural + C-channel + angles; up to 12.7" Ø × 27' long; 2D/3D bevel; AR-Tube design software
- **Bending** — Mitsubishi/Murata electric + hydraulic hybrid; robotic bending cell; parts to 330 lbs / 14ft long
- **Welding** — manual + robotic weld cells; in-house fixture design
- **Finishing** — partner network: plating, galvanizing, e-coat, powder coat
- **Engineering** — DFM, material selection, process selection
- **TODO (ask Henry):** Add laser wattage (kW), bed size, press brake tonnage — not currently published on site

### 3. Industries Served
- Trucking & Trailer (18-wheelers)
- Agricultural Equipment (tractors)
- Power Systems / Data Centers
- Industrial Equipment
- Each with a brief callout and relevant part photo

### 4. Photo Gallery
- All 20 shop photos in a masonry grid
- DSC01389 (I-beam stacks), DSC01618 (tube cross-sections) as anchors

### 5. Why DSW (differentiators)
- "Sales team with manufacturing experience" (this is actually good — keep it)
- Since 2004 (20+ years — credibility)
- QA verification process
- Automated quoting accuracy

### 6. Get Started (CTA section)
- Simple quote request form (name, email, file upload, notes)
- Phone + email + location

---

## Tech Stack Recommendation

**Astro + Tailwind CSS** — deployed on Vercel
- Fast, static output, easy to maintain
- Works great with image optimization (the photos are large JPEGs)
- Can add a contact form via Netlify Forms or Formspree
- Total build time: 2-3 days for full site

**Staging URL suggestion:** `dsw-preview.vercel.app`
(or `dsw-new.vercel.app` — easy to remember, change to real domain later)

---

## Copy Notes

Replace all generic phrases with specific ones:
| Old | New |
|-----|-----|
| "A Reliable Product Partner" | "Built for Uptime. Built for Industry." |
| "Latest Technology and Equipment" | "Fiber Laser Cutting. Tube Laser. CNC Press Brake." |
| "Competitive Pricing" | "Automated quoting — accurate prices, faster than a phone call" |
| "On-time fulfillment" | "Parts when you need them. Not when it's convenient for us." |

---

## Next Steps

1. Henry confirms site structure and headline direction
2. Vector builds Astro scaffold + Tailwind layout
3. Drop in photos from ~/projects/dsw-website/photos/
4. Write/finalize copy for each section
5. Deploy to Vercel staging URL
6. Henry reviews, approves, presents to boss
