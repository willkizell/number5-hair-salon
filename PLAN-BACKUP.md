# Number 5 Hair Salon — Plan Backup

> Recovered from the Claude session `1297742b-2949-41dc-ae2f-6dcdb4bddf23` (old repo path
> `~/integren-crm`, before the CRM moved to `~/Desktop/integren-crm`). This is the
> "make money plan" thread. Captured 2026-07-01.

---

## The deal
- **Client:** Number 5 Hair Salon — UBC AMS Nest, Vancouver (401-6133 University Blvd), ☎ 604-559-8998
- **Bilingual EN / 中文**, walk-ins. On UBC campus (same turf as the Hillel BC lead).
- **Price:** $400 MAX — William is fine with that.
- **Goal:** Walk into the shop **Thursday (Jul 2)** with EVERYTHING — documents, paperwork,
  outline, screenshots — hand it over, get paid $400, done.
- **Language reality:** The **owner does not speak English** (first language Mandarin). William
  has been talking through the store manager. So the handoff paperwork needs a **Mandarin copy**.

## Where it sits in the CRM
- Filed into the CRM the proper way: workspace folder + `CLIENT.md` → `import-workspace.mjs` →
  **Supabase `clients`** (the CRM's source of truth; filesystem is legacy).
- `MASTER_INDEX.md` row updated from stale "Prospect (cold)" to filed client.
- BlackFly Booze plan was also indexed (separate, on hold — focus moved to Number 5).

---

## The tech (as discovered)
- **Site:** one self-contained `index.html` (~92KB), content-driven by `data/*.json`
  (services, gallery, hero, settings). Photos in `/photos`. Bilingual via `data-en`/`data-zh`
  attributes + JSON `name_en`/`name_zh`.
- **CMS = Decap CMS** at `/admin`, via **Netlify Identity + Git Gateway**. Owner logs in,
  edits, Decap commits to git `main`, Netlify redeploys.
- **Repo:** GitHub `number5-hair-salon`, work merged/pushed to `main`. **Not on Netlify yet.**

### Key architecture finding
- ✅ Announcement bar, hero image, gallery → already hydrate from JSON → CMS-editable.
- ❌ **Services + prices were HARDCODED** in `index.html` (lines ~1665–1891). CMS "Services"
  panel pointed at `services.json` which the page **ignored**, and prices weren't in the CMS
  at all. Two disconnected systems.

---

## Work COMPLETED this session (all committed + pushed to `main`)
1. **Hero photo** — was silently 404'ing (`hero.json` pointed at a missing file, fell back to
   old interior shot). Fixed → storefront photo as `number5-storefront.jpg` (fresh filename,
   cache-busted). Note: hero photo is **intentionally hidden on mobile** (original design).
2. **Footer** — "Made by Integren" using the **clean white wordmark** (`integren-wordmark.png`,
   no filter), links to `integren.site`.
3. **Map** — widened (contact grid 5fr/7fr), rounded corners + soft shadow, un-greyscales on
   hover; mobile stacks full-width. Added a root-level `overflow-x: hidden` guard on `html`.
4. **Services + prices → fully CMS-editable, bilingual, add/remove.** All **11 services**
   migrated into `services.json` (Men's = single price; Lady = dual Short/Long). Accordion made
   re-initializable so it survives CMS DOM rebuilds; services now render from JSON.
5. Verified headlessly (Playwright): **zero horizontal overflow at 375→1920px**; hero flush to
   viewport edge. The scrollbar William saw = Mac "always show scrollbars" setting, not a bug.

### The 4 branded walk-in docs (built with `/integren-brand` skill)
- In `_legal/`: **Service Agreement** ($400, blank owner signature line) + **Invoice** #N5-001 ($400).
- In `_knowledge/`: **Handoff Summary** ("What You're Getting") + **CMS Quick-Start**
  (how to edit, incl. Mandarin).

---

## Remaining scope to be "done" (the 4 buckets)

**A. Make it live (the real gate — nothing works until this is done)**
- Deploy repo `number5-hair-salon` to **Netlify** (static site, no build, publish dir = root).
- Netlify → **Identity → Enable** → **Services → Git Gateway → Enable** → **invite owner's email**
  (this turns the CMS on).
- **Domain:** `number5hairsalon.com` on Namecheap (~C$15.45/yr, or **$6.79 w/ code NEWCOM679**).
  Then Netlify → Domain settings → add domain → point DNS per Netlify's records.

**B. Content truth (credibility risks)**
- **Translation QA** — native speaker to eyeball 中文 (announcement + services). Bad Mandarin in
  a Chinese-owned salon could kill the sale.
- **Real photos** — some services still used Unsplash stock; swap for real salon photos.
- **Confirm final services + prices** with owner (prices now supported in CMS).

**C. CMS onboarding (their "we want to edit it ourselves" requirement)**
- Test full loop once live: log in → change announcement → save → see it update.
- Printed one-page CMS quick-start in their hands.

**D. The walk-in package** — Agreement, Invoice, Handoff one-pager, CMS quick-start, printed
screenshots (desktop + mobile).

---

## ⬅️ LAST REQUEST (where the session cut off — NOT yet done)
> Make **copies of each doc fully in Mandarin** (owner's first language). Two copies each:
> **English + Mandarin**. Include in the docs:
> - **Screenshots of the site** showing him what it is
> - **Screenshots of how to use it** (the CMS)
>
> Reason: owner doesn't speak English; William wants to hand him paper he can actually read —
> contracts AND outlines.

**→ This is the next action to pick up.**

---

## Parked: BlackFly Booze (separate thread, on hold)
- Target: **Black Fly Beverages** (blackflybooze.com), family-run RTD cocktail brand, London ON.
- Prototype already built at `~/Integren Sites/BlackFly Booze/` (repo `willkizell/blackfly.git`),
  consolidated (strays folded in, early one-pager archived).
- Way in: **Owen Kelly** (Toronto sales rep, family) → carries to **Cathy Siskind-Kelly**
  (co-founder / decision-maker). ⚠️ Owen's LinkedIn is a ghost account — don't rely on the DM;
  prefer company email/phone (London ON, (519) 667-1555) attn: Cathy.
- Ask: $15k, $7.5k upfront.
- Accountability calendar (wkizell@gmail.com): daily 8am nudge + checkpoints through Jul 14.
