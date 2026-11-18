# Partner Newsletter — Offer catalog working sheet (Distributor example)

**Audience:** DA + Partner newsletter authors organizing Decisioning inventory.  
**Role:** Translate the team’s offer spreadsheet into AJO Decisioning catalog fields.  
**Parent docs:** [`decisioning-quick-reference.md`](decisioning-quick-reference.md) · [`decisioning-items-offers-requirements.md`](decisioning-items-offers-requirements.md)  
**Team Guide:** [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md) §11

**Status:** Working inventory from the Distributor Newsletter sheet (example). Schema not yet built in AJO. Language deferred from initial pilot per sheet.

**Source PDF:** [`../../sources/AJO_EXAMPLE_Distributor_Newsletter_Offer_Catalog.pdf`](../../sources/AJO_EXAMPLE_Distributor_Newsletter_Offer_Catalog.pdf)

---

## Why this helps

The sheet is an **AJO Offer Catalog** intake form for one newsletter section (**Distributor**). That is the right unit of work for Decisioning:

- Each row ≈ one **decision item**
- Columns ≈ catalog attributes + eligibility + priority + dates + tags + placement intent
- Filling Distro first proves the model before Partner Pulse / TechDev sheets

---

## Sheet columns → Decisioning

| Sheet column | Maps to in AJO | Notes |
|---|---|---|
| Offer Title / ITEM NAME | Decision item **Name** (+ optional display title custom attr) | Keep names stable for reporting |
| Offer Description | Custom attr `body` / `description` (**Decisioning Content** if personalized) | Long-form offer copy |
| Offer CTA | Custom attr `ctaLabel` (String or Decisioning Content) | Button / link text |
| Offer CTA Link | Custom attr `ctaUrl` (String) | Prefer Decisioning Asset only if you need tracked asset URLs |
| Region | Custom attr `region` (String) | Sheet examples use `Global`; eligibility or collection filter |
| Partner | Custom attr `partnerType` or audience | Sheet: `Distributor` — drives Distro collection / eligibility |
| Admin Only? | **Gap** | Sheet note: not available; only queryable via Workbench in RHSC — do not put in v1 catalog until profile path exists |
| Persona | Custom attr `persona` (String) — optional later | Blank / deferred in Distro example |
| Language | Custom attr `language` | Sheet: **not in initial pilot** |
| Partner Module | Custom attr `partnerModule` (String) | Enablement groupings (see inventory below) |
| Priority | Item **Priority** (integer) | Sheet bands: Required **100** · High **50–75** · Medium **26–50** · Low **0–25** |
| Date Range | Item **start / end dates** | Required for in-date delivery |
| Notes | Item description / internal notes | Not rendered unless you add a field |
| Tag | Item **tags** +/or custom attrs | Sheet theme tags: Partner · Product · Region · Channel · Destination/System |
| Placements | Email **placement** assignment (not an item field) | Create named placements in Strategy setup → Email; leave blank on sheet until slot design is fixed |
| Newsletter Section | Collection rule / `newsletterProgram` | This sheet = `distributors` |

---

## Recommended catalog attrs (Distro v1)

Aligns the sheet with the Decisioning Offers schema (extend `_YourOrg`):

| Attribute | Type | From sheet |
|---|---|---|
| `newsletterProgram` | String | `distributors` (Pulse / TechDev later) |
| `partnerType` | String | `Distributor` |
| `partnerModule` | String | Partner Enablement, Marketing Enablement, etc. |
| `region` | String | e.g. `Global` |
| `headline` / `title` | Decisioning Content or String | Offer Title |
| `body` | Decisioning Content | Offer Description |
| `ctaLabel` | String / Decisioning Content | Offer CTA |
| `ctaUrl` | String | Offer CTA Link |
| `language` | String | **Defer** until post-pilot per sheet |

**Do not invent in v1:** Admin Only (no AJO-queryable path yet).

**Tags (exact values for collections):** use discrete tags — `CONTAINS` is exact `IN`, not substring.

---

## Priority convention (from sheet)

| Band | Integer guidance | Use |
|---|---:|---|
| Required | 100 | Must-win / time-bound program update |
| High | 50–75 | Strong enablement / pipeline plays |
| Medium | 26–50 | Secondary plays |
| Low | 0–25 | Evergreen / soft CTAs |

Keep scores **unique within a competing set** when ranking = offer priority.

---

## Distro example inventory (from sheet)

These are discovery examples for sandbox items — not yet approved in AJO.

| Item name (suggested) | Priority band | Partner module / section | CTA | Date range (sheet) |
|---|---|---|---|---|
| `DIST-Aligned-Distribution-2027` | Required (100) | Partner Enablement / Partner Program Updates | Aligned Distribution How-To Guide | Oct 1 – Nov 30, 2026 |
| `DIST-RHEL-Security-Initiative-4pct` | High (50–75) | RHEL Sales + Marketing Enablement | Turn security into pipeline | Oct – Dec 2026 |
| `DIST-Partner-Demand-Center-YE` | Low (0–25) | Partner Enablement / Marketing Enablement | Email concierge for demo | All year |
| `DIST-Ecosystem-Awards-2027` | Low (0–25) | Partner Program Updates / Marketing Enablement | Connect with DPAM | (nomination cycle) |
| `DIST-Red-Hat-AI-3-5` | High (50–75) | AI Sales + Marketing Enablement | Learn more | All year |
| `DIST-Cisco-Ansible-Playbook` | Medium (26–50) | Ansible Sales Enablement | Access and Share the Playbook | (sheet blank) |

**Collection sketch:** `newsletterProgram equals distributors` (and optionally `partnerType equals Distributor`).

**Eligibility sketch:** Audience = Distributor partner membership (stable); add region/module rules only when profile fields are reliable.

**Placement sketch:** `Email-Hero-Partner-Newsletter` (or Distro-specific `Email-Hero-Distributor-Newsletter` if separate templates).

---

## Gaps / decisions the sheet surfaces

| Gap | Implication |
|---|---|
| **Placements column empty** | Design email slot(s) before sandbox build; placements are AJO objects, not spreadsheet cells |
| **Language not in initial pilot** | Schema can reserve `language`; don’t block Distro v1 on LocSync |
| **Admin Only not in AJO** | Exclude from eligibility until a profile attribute exists |
| **Pulse / TechDev sheets** | Same column model; separate collections + `newsletterProgram` values |
| **Multi-program partners** | Still need program-scoped vs unified policy decision (requirements doc §6.3) |

---

## How to use with the team

1. Keep the **spreadsheet as authoring intake** (owners fill rows).  
2. Treat this markdown as the **contract** for which columns become catalog fields.  
3. Build Distro sandbox first: schema → 3–6 approved items from the table above → 1 collection → 1 strategy → 1 placement → 1 policy.  
4. Clone the sheet structure for Partner Pulse and TechDev; do not invent a second catalog.

---

**Last updated:** 2026-11-18
