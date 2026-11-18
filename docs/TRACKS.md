# AJO B2C — two working tracks

Use this when work feels scattered. Everything in the binder maps to **one** of these (or an explicit side track).

**Human start (enablement):** [`AJO-B2C-Team-Guide.md`](AJO-B2C-Team-Guide.md) — how we work in AJO B2C (incl. §10 IT / process materials). Dense stack contracts stay in handbook Chapter 2.  
**Team git:** [DX-Architecture/AJO-B2C-Config](https://github.com/DX-Architecture/AJO-B2C-Config) `docs/` (markdown share SoT; keep aligned with this binder).

| Track | Job | Home | Not this track |
|---|---|---|---|
| **1. Platform expansion** | What to learn / recommend **after** pilot signal — features, Focus Areas bets, parking-lot ideas | [`platform-analysis-parking-lot.md`](platform-analysis-parking-lot.md) | Day-to-day journey build checklists |
| **2. Journey build requirements** | How to **author** a journey: audience type → entry shape → panel settings for **promotional** vs **transactional** | [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md) | Feature backlog / OpenShift ideation |

**Side tracks (keep out of both until merged on purpose)**
- LocSync / Multilingual → [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md)
- Decisioning depth → [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md)
- Pilot narrative / scorecard → [`project-brief.md`](project-brief.md)

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

**Start:** [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md)  
**Deep promo settings:** [`ops/promotional-read-audience-settings.md`](ops/promotional-read-audience-settings.md) → handbook §2.5.2  
**Deep transactional settings:** [`ops/transactional-unitary-event-settings.md`](ops/transactional-unitary-event-settings.md) (pilot unitary / MARTECHCBP-9868)  
**Entry model why:** parking lot **PA-04** (analysis) — build checklist lives in track 2, not the parking lot  
**Handbook:** Digital Architecture Handbook Ch2 (not in this repo) §2.2 (types) · §2.5.2 (promo Read Audience)

| Comm class | Typical RH example | Entry |
|---|---|---|
| **Promotional** (scheduled nurture) | Learn Deep / OpenShift pre-purchase shell | **Read Audience** |
| **Promotional** (streaming fallout) | Abandonment (if AQ) | **Audience Qualification** |
| **Transactional** | Trial Start Welcome / Expiring / Survey | **Unitary event** |

---

**Last updated:** 2026-11-18
