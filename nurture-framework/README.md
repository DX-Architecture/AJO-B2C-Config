# Nurture Framework × AJO B2C

How the **CY26 Omnichannel Nurture Framework** (stages, Experience Patterns, Nets / Ladders / Catapults) maps onto **AJO B2C** execution — vs the current **Marketo**-centric architecture.

**Rule:** Keep the framework as the operating model. Use AJO B2C to elevate *how* patterns run (entry, exit, real-time, decisioning, arbitration) — do not rebuild Discover→Expand as Marketo clones inside AJO.

## Start here

| Topic | Notes |
|---|---|
| Stages | Discover → Learn → Evaluate → Adopt → Expand |
| Experience Patterns | Objective · Audience (catch) · KPIs (release) · content mix / destinations / messaging |
| One pattern per product line | Dual-active messaging journeys on the same product line violate the framework unless intentional |
| Digital tactics | Chat · Email · Site personalization · Paid media (Digital Architecture foundation) |

## Marketo vs AJO B2C (short)

| Dimension | Marketo today | AJO B2C elevation |
|---|---|---|
| Primary job | Lifecycle email + CRM-connected demand | Real-time / profile-centric journeys on AEP |
| Nurture shape | Program / stream per product or topic | Journey shell per Experience Pattern |
| Entry | Smart lists / triggers | Read Audience (ladders) · AQ (nets) · unitary (catapults) |
| Variants | Clone programs | Dynamic content / Decisioning offer slot |
| Conflicts | Marketo-only caps | Channel + journey caps — **not** aware of Marketo |
| Measure | Program reports | CJA: AJO + web + Marketo baselines |

## Architecture guides (this folder)

| Doc | Purpose |
|---|---|
| [`marketo-vs-ajo-architecture.md`](marketo-vs-ajo-architecture.md) | Dual-runtime coexistence + excludes · pain → AJO capability |
| [`experience-pattern-migration-order.md`](experience-pattern-migration-order.md) | Which patterns move to AJO first |
| [`nets-ladders-catapults-ajo.md`](nets-ladders-catapults-ajo.md) | Framework pillars → AJO journey types |

## Related docs in this repo

- [`../docs/TRACKS.md`](../docs/TRACKS.md) — platform expansion vs journey build
- [`../docs/AJO-B2C-Team-Guide.md`](../docs/AJO-B2C-Team-Guide.md) — how we author in AJO B2C
- [`../docs/architecture/journey-build-requirements.md`](../docs/architecture/journey-build-requirements.md) — promo vs transactional settings
- [`../docs/architecture/decisioning-items-offers-requirements.md`](../docs/architecture/decisioning-items-offers-requirements.md) — Decisioning + framework elevation depth
- [`../docs/ops/platform-analysis-parking-lot.md`](../docs/ops/platform-analysis-parking-lot.md) — PA-03 Jump, PA-04 entry model, PA-08 exit criteria
