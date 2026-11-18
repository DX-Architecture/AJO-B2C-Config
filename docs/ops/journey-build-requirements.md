# Journey build requirements — audience types & journey settings

**Track:** 2 — Journey build (not platform expansion)  
**Home index:** [`../TRACKS.md`](../TRACKS.md) · enablement overview [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md)  
**Status:** Working contract — consolidate promo + transactional build rules here; park feature ideas in the parking lot.

**Evidence / SoT**
- Promo Read Audience panel: [`promotional-read-audience-settings.md`](promotional-read-audience-settings.md) · screenshot `ajo-read-audience-rhel-learn-deep.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo)
- Transactional unitary event: [`transactional-unitary-event-settings.md`](transactional-unitary-event-settings.md) · screenshot `ajo-unitary-event-pilot-transactional.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo)
- Handbook: Digital Architecture Handbook Ch2 (team Google Doc / local sync; not in this repo) §2.2 (journey types) · §2.5.2 (promo contract)
- Pilot shapes: [`../project-brief.md`](../project-brief.md) (Abandonment nurture + Trial Start transactional)
- Entry analysis (why Read Audience ≠ Jump): parking lot **PA-04**

---

## 1. Pick the comm class first

| Comm class | Job | RH examples | Default entry |
|---|---|---|---|
| **Promotional — scheduled nurture** | Who ∩ when on a standing cadence | Learn Deep, OpenShift pre-purchase shell, Partner newsletter shell | **Read Audience** |
| **Promotional — streaming fallout** | Enter when membership flips | Abandonment (if AQ on CLM segment) | **Audience Qualification** |
| **Transactional** | Real-time person event → fixed sequence | Trial Start Welcome / Expiring Soon / Survey | **Unitary event** |

Do **not** choose entry from the email calendar (“we send Tuesdays”) — choose from the **signal** (batch membership vs stream join vs person event). Handbook §2.2.

---

## 2. Audience / signal requirements by class

### A. Promotional — scheduled nurture (Read Audience)

| Requirement | Contract |
|---|---|
| **Audience** | AEP **Persons** audience for the Experience Pattern. Membership should already encode stage / excludes the business needs. |
| **Marketable** | Still condition `profile.b2b._redhat.marketable == true` inside the journey when that gate is required. |
| **Namespace** | Must match person identity on the profile. RHEL promo reference: **B2B Person (`b2b_person`)**. Wrong namespace = silent non-entry. |
| **Identity family** | Same as Marketo→AEP B2B Person map (`b2b._redhat.*`) for Persons promo paths. |
| **Jump** | **Not available** on this journey (cannot contain Jump; cannot be Jump target). |

### B. Promotional — streaming fallout (Audience Qualification)

| Requirement | Contract |
|---|---|
| **Audience** | Named streaming-capable segment (pilot: `CLM - RHEL RHPT Abandonment (2025 update)`). |
| **Time window** | Put in **segment** *or* canvas AND — pick one; write it on the journey. |
| **Latency** | ~15–30 min Edge→Hub; not true real-time. True real-time → unitary. |
| **Namespace** | Same rule: match profile identity used at entry. |
| **Jump** | AQ → event Jump is progressive / validate in sandbox; do not assume. |

### C. Transactional (Unitary event)

| Requirement | Contract |
|---|---|
| **Signal** | Person-level XDM experience event (rule-based or system-generated eventID). |
| **Identity on payload** | Required so AJO stitches to profile (same namespace the journey listens on). |
| **Audience membership** | Not the entry gate — event is. Optional conditions after entry. |
| **Catalog** | Event must be named in the event catalog before scaling (handbook §2.3 — still a gap for some pilot signals). |
| **Jump** | Allowed only with compatible event-based shapes + same namespace; prefer **exit nurture + unitary entry** for Trial Start (see PA-03 / PA-08). |

---

## 3. Journey / activity settings checklist

### Shared (all classes)

| Setting | What to decide |
|---|---|
| Journey timezone | RH standard (journey TZ drives weekday/quiet evaluation) |
| Re-entrance | Explicit policy (promo nurture often restricted; transactional often once-per-event design) |
| Exit criteria | Promo: release on conversion (e.g. trial started) — **PA-08**. Transactional: usually natural end after sequence |
| Channel business rules | Promo: marketing frequency + quiet hours candidates (**PA-02**). Transactional: **exclude** from quiet-hours / marketing caps unless CX says otherwise |
| Tags | Milestone name on activity; Unified Tag categories per [`aep-cdp-ajo-b2c-tag-structure-sketch.md`](aep-cdp-ajo-b2c-tag-structure-sketch.md) |

### Promotional — Read Audience panel + schedule

| Setting | Required / default (RH promo) |
|---|---|
| Label | Customer-milestone name (e.g. RHEL Learn Deep) |
| Audience | Pattern Persons audience |
| **Namespace** | `b2b_person` (Persons promo path) |
| Merge policy | Default Timebased unless documented exception |
| Supplemental identifier | **Off** (standard one entrance per execution) |
| Reading rate | Start **5,000**/sec; sandbox shares **20,000**/sec across concurrent Read Audiences |
| Journey schedule | **Daily** for standing nurture (not one-time blast) |
| **Incremental read** | **On** for daily — net-new joiners only |
| Trigger after batch audience evaluation | Prefer when audience is batch-evaluated |

Full paste + Learn Deep reference: [`promotional-read-audience-settings.md`](promotional-read-audience-settings.md).

### Transactional — unitary journey

| Setting | Required / default (RH pilot unitary — MARTECHCBP-9868) |
|---|---|
| Entry | **Unitary** event (not Read Audience) |
| Event id type | **Rule based** (pilot) — condition on payload |
| Schema | B2B Activity v.1 |
| Fields | 9 selected — click the **eye** at the top of the event to show all |
| Event id condition | `campaignInteraction_c.offerID` equals campaign offer ID (pilot: `7016e000001cj7JAAU`) |
| Namespace | **B2B Person (`b2b_person`)** |
| Profile identifier | `personKey.sourceKey` |
| Merge policy | Default Timebased |
| Supplemental identifier | **Off** |
| Schedule | Event-driven — **no** daily Read Audience schedule / incremental read |
| Quiet hours / marketing rule set | Do **not** apply promo marketing governance by default |
| Dual entry | Prefer **unitary only** for Trial Start (not Jump-from-nurture) |

Full paste + screenshot: [`transactional-unitary-event-settings.md`](transactional-unitary-event-settings.md).

---

## 4. Side-by-side (build view)

| Topic | Promotional (scheduled) | Transactional |
|---|---|---|
| Who | Audience membership (+ marketable) | Person who fired the event |
| When | Journey schedule (daily + incremental) | When event is ingested |
| Entry activity | Read Audience | Unitary event |
| Namespace | `b2b_person` (Persons promo ref) | `b2b_person` (pilot unitary) |
| Jump on this journey | No | Possible (event↔event only; use sparingly) |
| Typical exit | Exit criteria / conversion event | End of email sequence |
| Caps / quiet hours | Channel marketing rule set candidate | Usually exempt |
| Golden reference | RHEL Learn Deep Read Audience | Pilot unitary event (MARTECHCBP-9868) |

---

## 5. Gaps to close (build track only)

- [ ] Confirm live Abandonment = AQ vs Read Audience (handbook still notes both shapes)
- [x] Pilot unitary event settings captured (rule-based, B2B Activity v.1, `b2b_person`, offerID condition) — [`transactional-unitary-event-settings.md`](transactional-unitary-event-settings.md)
- [x] **R&R:** New unitary events (schema / dataset / streaming / event config) are **built by IT** — DA authors request via template; do not treat missing dataset IDs in Journeys payload samples as a DA fill task
- [ ] Written rule: which Channel rule sets attach to promo vs transactional email actions
- [ ] Sync this contract into handbook §2.5.x when stable; then Q1 BP pack ([`ajo-permissions-user-onboarding-q1-sketch.md`](ajo-permissions-user-onboarding-q1-sketch.md))

---

**Created:** 2026-11-18  
**Last updated:** 2026-11-18 — transactional unitary reference from sandbox screenshot
