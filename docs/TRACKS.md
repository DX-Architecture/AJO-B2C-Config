# AJO B2C — two working tracks

Use this when work feels scattered. Everything in the binder maps to **one** of these (or an explicit side track).

**Human start (enablement):** [`AJO-B2C-Team-Guide.md`](AJO-B2C-Team-Guide.md) — how we work in AJO B2C (incl. §10 IT / process materials). Dense stack contracts stay in handbook Chapter 2.  
**Team git:** [DX-Architecture/AJO-B2C-Config](https://github.com/DX-Architecture/AJO-B2C-Config) `docs/` (markdown share SoT; keep aligned with this binder).

| Track | Job | Home | Not this track |
|---|---|---|---|
| **1. Platform expansion** | What to learn / recommend **after** pilot signal — features, Focus Areas bets, parking-lot ideas | [`ops/platform-analysis-parking-lot.md`](ops/platform-analysis-parking-lot.md) | Day-to-day journey build checklists |
| **2. Journey build requirements** | How to **author** a journey: audience type → entry shape → panel settings for **promotional** vs **transactional** | [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md) | Feature backlog / OpenShift ideation |

**Side tracks (keep out of both until merged on purpose)**
- LocSync / Multilingual → [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md)
- Decisioning → [`architecture/decisioning-quick-reference.md`](architecture/decisioning-quick-reference.md) (offer / copy / placement) · depth [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md)
- Pilot narrative / scorecard → [`RHEL-Trial-Experience.md`](RHEL-Trial-Experience.md)
- CX Coworker (AJO B2C prompts / custom skills) → [`ops/cx-coworker-ajo-b2c.md`](ops/cx-coworker-ajo-b2c.md)

**Canvas (track 2 at a glance):** `ajo-b2c-journey-build-requirements.canvas.tsx` (open beside chat)

---

## Track 1 — Platform expansion

**Start:** parking lot backlog (PA-01…08, OPS-01…03).  
**Graduate when:** Focus Areas use-case write-up or explicit defer.  
**Q4 / CY27 map:** section “Focus Areas alignment” inside the parking lot.

| Priority now (expansion) | ID |
|---|---|
| Decisioning + OpenShift dynamic content | PA-06 (+ architecture note) |
| Pattern hygiene: entry + exit + caps | PA-04, PA-08, PA-02 |
| Jump only after dual-activity rules | PA-03 |
| Personalization inventory | PA-05, OPS-02 |

---

## Track 2 — Journey build requirements

**Start:** [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md)  
**Deep promo settings:** [`architecture/promotional-read-audience-settings.md`](architecture/promotional-read-audience-settings.md) → handbook §2.5.2  
**Deep transactional settings:** [`architecture/transactional-unitary-event-settings.md`](architecture/transactional-unitary-event-settings.md) (pilot unitary / MARTECHCBP-9868)  
**Entry model why:** parking lot **PA-04** (analysis) — build checklist lives in track 2, not the parking lot  
**Handbook:** Digital Architecture Handbook Ch2 (not in this repo) §2.2 (types) · §2.5.2 (promo Read Audience)

| Comm class | Typical RH example | Entry | Exit criteria |
|---|---|---|---|
| **Promotional** (scheduled nurture) | Learn Deep / OpenShift pre-purchase shell | **Read Audience** | **Required** if release KPI (build §4) |
| **Promotional** (streaming fallout) | Abandonment (if AQ) | **Audience Qualification** | **Required** (e.g. trial started) |
| **Transactional** | Trial Start Welcome / Expiring / Survey | **Unitary event** | Usually natural end of sequence |

---

**Last updated:** 2026-11-18
