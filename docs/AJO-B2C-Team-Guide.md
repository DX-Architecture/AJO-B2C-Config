# AJO B2C — Team Guide

**Audience:** Digital Architecture teammates new(er) to AJO B2C (journey authors, Q1 cohort).  
**Role:** Enablement / how we work day to day.  
**Not this doc:** Stack-wide load-bearing contracts live in the **team handbook** Chapter 2 (Digital Architecture Handbook Ch2 (not in this repo)). When this guide and the handbook disagree on a contract, **handbook wins** until you deliberately refresh Ch2.

**Binder index:** [`TRACKS.md`](TRACKS.md) · Pilot narrative: [`project-brief.md`](project-brief.md)

---

## 1. Purpose vs handbook

| Artifact | Use it for |
|---|---|
| **This guide** | Which journey shape, which settings, where to click next, onboarding path |
| **Handbook Ch2** | System of record, runtime contracts, limits, failure modes, CJA join, debug |
| **Track 2 ops** | Detailed promo vs transactional build checklists |
| **Parking lot** | Platform expansion ideas — not build SOPs |

Google Doc paste later is fine; **working SoT for this guide is this markdown file.**

---

## 2. Two tracks (don’t mix)

| Track | Job | Start here |
|---|---|---|
| **1. Platform expansion** | Features / Focus Areas bets after pilot signal | [`platform-analysis-parking-lot.md`](platform-analysis-parking-lot.md) |
| **2. Journey build** | Audience type → entry → panel settings | [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md) |

Full map: [`TRACKS.md`](TRACKS.md).

**Side tracks (optional later):** Decisioning ([quick ref](architecture/decisioning-quick-reference.md)) · LocSync / Multilingual — not required to ship a first promo or transactional journey.

---

## 3. Journey types cheat sheet

Pick the type from the **entry signal**, not from the email calendar. Dense contract: handbook **§2.2**.

| Type | Enters when | RH use |
|---|---|---|
| **Unitary** | Person-level experience event | Transactional Trial Start; conversion exit signal |
| **Read Audience** | Scheduled batch read of a Persons audience | Standing promotional nurture (Learn Deep–style) |
| **Audience Qualification** | Profile joins/leaves a segment (near-real-time) | Abandonment-style fallout if AQ is the live qualifier |
| **Business event** | One business condition → many profiles | Not on RHEL pilot; rare |

Latency reminder: AQ is ~15–30 min Edge→Hub. True real-time needs **unitary**.

---

## 4. Build: promotional vs transactional

Author checklist SoT: [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md).  
Promo Read Audience panel detail: [`architecture/promotional-read-audience-settings.md`](architecture/promotional-read-audience-settings.md) · handbook **§2.5.2**.

| Comm class | Default entry | Namespace (Persons promo ref) | Schedule |
|---|---|---|---|
| **Promo — scheduled nurture** | Read Audience | `b2b_person` | **Daily** + **incremental read** |
| **Promo — streaming fallout** | Audience Qualification | Match profile identity | Event/membership driven |
| **Transactional** | Unitary event | `b2b_person` (pilot) | Event ingest (no daily / incremental read) |

**Marketable gate (when required):** `profile.b2b._redhat.marketable == true` inside the journey.  
**Wrong namespace on Read Audience** → profiles can fail to enter with little authoring error (handbook contract).

**Jump:** Not on Read Audience journeys. Prefer **exit nurture + unitary entry** for Trial Start (see §7).

---

## 5. Golden paths (pilot)

See [`project-brief.md`](project-brief.md).

### Product Trial Abandonment (nurture / promo-shaped)

- Catch SSO / registration fallout before download.  
- Audience example: `CLM - RHEL RHPT Abandonment (2025 update)`.  
- Put time window in **segment or** canvas AND — pick one and write it down.  
- Exit when trial actually started (unitary / exit criteria) — do not treat Start Click as activation.

### Product Trial Start (transactional)

- Welcome / Expiring Soon / Survey on **unitary** entry.  
- Pilot event reference: **rule-based** unitary on schema **B2B Activity v.1**, namespace **`b2b_person`**, profile id `personKey.sourceKey`, condition on `campaignInteraction_c.offerID` — [`architecture/transactional-unitary-event-settings.md`](architecture/transactional-unitary-event-settings.md).  
- Do **not** Jump from a Read Audience nurture shell into Trial Start as the primary design.  
- Keep marketing quiet hours / freq caps **off** transactional actions unless CX explicitly wants them.

---

## 6. Identity, namespace, marketable

| Topic | Practice |
|---|---|
| Promo Persons path | Namespace **B2B Person (`b2b_person`)** — Learn Deep reference |
| Transactional unitary (pilot) | Same **`b2b_person`** · id = `personKey.sourceKey` · rule-based offerID condition — [txn settings](architecture/transactional-unitary-event-settings.md) |
| Event journeys | Payload must carry a person identity AJO can stitch; same namespace family as the journey |
| Marketable | Verified path: `profile.b2b._redhat.marketable` |
| Fields inventory | [`architecture/aep-email-personalization-fields.md`](architecture/aep-email-personalization-fields.md) |
| Business Rules / capping | Prefer highest-priority people namespace when caps are attached (Adobe warning) — separate from silent non-entry |

---

## 7. Exit criteria + Jump (short)

**Exit criteria is a journey build requirement** for promo/nurture with a release KPI — full contract: [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md) **§4**.

| Topic | Rule of thumb | Depth |
|---|---|---|
| **Exit criteria** | Promo/nurture: **required** stop when purpose is met (Event / Audience / Profile attribute) | Build §4 · parking lot **PA-08** |
| **Jump** | Fork (can stay active in A+B) — not a clean handoff | **PA-03** |
| **Scheduled nurture → Trial Start** | Exit A + unitary entry to B — do not Jump from Read Audience | **PA-04** |

---

## 8. Caps / quiet hours

| Comm class | Channel business rules |
|---|---|
| **Promotional** | Candidate for marketing frequency capping + quiet hours (same Channel rule set) — parking lot **PA-02** |
| **Transactional** | **Exclude** from marketing quiet hours / caps by default |

AJO and Marketo caps are **not** aware of each other — coexistence still needs excludes (handbook §2.6 / Focus Areas).

---

## 9. Measurement (CJA)

Pilot success is not “journey finished” in AJO reports. Join sends to people/sessions/events in **CJA** (handbook **§2.7** and Chapter 6).

- Name every send node for diagnosability.  
- Confirm whether AJO sends land on the same Marketo-shaped metrics or a different feedback dataset.  
- Reporting “trial started” in CJA ≠ the AEP unitary event that gates the journey (handbook §2.4).

---

## 10. Ways of working with IT and process

Materials authors need to get started (best practices, journey setup examples). Source checklist from Q1 permissions / onboarding — depth and status: [`ops/ajo-permissions-user-onboarding-q1-sketch.md`](ops/ajo-permissions-user-onboarding-q1-sketch.md) · screenshot `sources/ajo-q1-materials-to-get-started.png`.

1. **Review AJO B2C & B2B user permissions** — confirm they support the UCs needed (with IT / Adobe admin).  
2. **Identify who needs platform access** ahead of Q1 (B2C cohort named in the sketch; B2B list still TBD).  
3. **What materials do they need to get started?** (best practices, examples of how to set up a journey)
   - **a.** Proof of cohort participation *(Co-Author)*  
   - **b.** Initial Best Practices *(Co-Author)*  
     - **i.** Marketable — `profile.b2b._redhat.marketable` ([fields inventory](architecture/aep-email-personalization-fields.md))  
     - **ii.** Bouncebacks  
     - **iii.** Journey entry guidance — promo vs transactional ([`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md))  
     - **iv.** Namespace guidance — Persons / unitary path `b2b_person`  
     - **v.** Audience selection *[Amanda Machado]*  
     - **vi.** Where / how to request a unitary event (including template) — **IT builds** new unitary events (schema / dataset / streaming / config); DA requests + uses. Pilot reference: [`architecture/transactional-unitary-event-settings.md`](architecture/transactional-unitary-event-settings.md)  
     - **vii.** How to request support  
   - **c.** Troubleshooting documentation for MarTech  

Until those BP pages exist, use this guide + handbook Ch2 + the two reference configs (Learn Deep promo · Pilot unitary).

---

## 11. Ops pack (links)

| Need | Doc |
|---|---|
| Promo Read Audience settings | [`architecture/promotional-read-audience-settings.md`](architecture/promotional-read-audience-settings.md) |
| Transactional unitary settings | [`architecture/transactional-unitary-event-settings.md`](architecture/transactional-unitary-event-settings.md) |
| Full promo vs txn build contract | [`architecture/journey-build-requirements.md`](architecture/journey-build-requirements.md) |
| Tags / CDP naming | [`ops/aep-cdp-ajo-b2c-tag-structure-sketch.md`](ops/aep-cdp-ajo-b2c-tag-structure-sketch.md) |
| Personalization field paths | [`architecture/aep-email-personalization-fields.md`](architecture/aep-email-personalization-fields.md) |
| Q1 permissions / IT & process sketch | [`ops/ajo-permissions-user-onboarding-q1-sketch.md`](ops/ajo-permissions-user-onboarding-q1-sketch.md) |
| **Decisioning — where offer / copy / placement live** | [`architecture/decisioning-quick-reference.md`](architecture/decisioning-quick-reference.md) |
| Decisioning — full requirements (items, setup, placements, use cases) | [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md) |

Destination tag values in play: Marketo · Target · AJO-B2C · AJO-B2B · CJA.

---

## 12. Side tracks (not first-journey blockers)

| Track | When to open |
|---|---|
| Decisioning (items / offers / Partner newsletter / Event Modernization) | Start [`architecture/decisioning-quick-reference.md`](architecture/decisioning-quick-reference.md) · depth [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md) — not required for a first promo/txn journey |
| LocSync / Multilingual | [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md) — separate from platform expansion parking lot |
| OpenShift pre-purchase / SKUs | Parking lot **PA-06** |

---

## 13. Propose a new use case

1. Use the Focus Areas **Why / What / How / Dependencies / DA Lead** template (workflow project / Focus Areas deck).  
2. If it’s a **build** pattern (entry, namespace, caps) → harden in track 2 (`journey-build-requirements` + handbook).  
3. If it’s a **platform feature bet** → park in [`platform-analysis-parking-lot.md`](platform-analysis-parking-lot.md).  
4. Do not grow one-off journey folklore only in chat or slides.

---

## Quick start (first week)

1. Read handbook Ch2 §2.2 + §2.5.2 (contracts).  
2. Skim this guide §§3–6 and §10 (IT / process materials).  
3. Open Learn Deep promo reference: [`architecture/promotional-read-audience-settings.md`](architecture/promotional-read-audience-settings.md).  
4. Open pilot unitary transactional reference: [`architecture/transactional-unitary-event-settings.md`](architecture/transactional-unitary-event-settings.md).  
5. Before publish: namespace · marketable · exit · Channel rule set (promo vs txn).

---

**Created:** 2026-11-18  
**Last updated:** 2026-11-18 — §11 Decisioning quick reference + full requirements as Team Guide resources
