# Marketo vs AJO B2C — dual-runtime architecture

**Purpose:** Define how the Omnichannel Nurture Framework runs when **Marketo** and **AJO B2C** both send.  
**Related:** [`README.md`](README.md) · [`nets-ladders-catapults-ajo.md`](nets-ladders-catapults-ajo.md) · [`experience-pattern-migration-order.md`](experience-pattern-migration-order.md) · [`../docs/AJO-B2C-Team-Guide.md`](../docs/AJO-B2C-Team-Guide.md)

---

## Elevation thesis

Do **not** replace the framework with “AJO campaigns.” Keep stages, Experience Patterns, catch/release, and **one pattern per product line**. Change the execution layer under email (and related digital tactics) so Marketo is no longer the only place Nets, Ladders, and Catapults can run.

**Pilot soundbite:** GenStudio builds the message · AJO delivers the moment · CJA proves the outcome.

---

## Current vs target stack

### Marketo-centric (current for most patterns)

```text
Experience Pattern
  → Marketo email nurture program (often one stream per product/topic)
  → parallel Adobe Target PZN (process-coordinated)
  → optional Intercom / chat
  → CRM / scoring handoffs
```

**Strengths:** Scheduled promotional cadence, smart-campaign ops, CRM-linked demand.  
**Gaps:** True real-time unitary moments, shared offer catalogs across patterns, cross-journey arbitration with Target telling the same story.

### Dual-runtime (target)

```text
Experience Pattern
  → AEP audience / unitary event
  → AJO B2C journey (promo Read Audience | AQ fallout | transactional unitary)
  → GenStudio content
  → optional Decisioning offer slot
  → Target on-site mirror (same moment/theme)
  → CJA vs Marketo baselines
```

**Marketo keeps:** Broad Discover / topic streams, CRM-heavy / sales-alert programs, interim coverage while an AJO pattern is in pulse-check.  
**AJO owns:** Learn–Adopt nets & catapults that need real-time entry, clean exit, decisioned content, and journey capping.

---

## Dimension comparison

| Dimension | Marketo (current) | AJO B2C (elevate) |
|---|---|---|
| Primary job | Lead/lifecycle email + CRM-connected demand | Real-time / profile-centric journeys on AEP |
| Audience model | Smart lists / membership inside Marketo | RT-CDP Persons audiences + unitary events (`b2b_person`) |
| Nurture shape | Program / engagement stream per product or topic | Journey shell per Experience Pattern; variants via content/Decisioning |
| Real-time | Trigger campaigns; still campaign-centric | Unitary for Trial Start–class; AQ (~15–30 min) for fallout |
| Omnichannel | Email strong; web PZN separate | Email + Target can share moment/offer theme; CJA joins channels |
| Personalization | Tokens / snippets / clones for big variations | Profile Handlebars + dynamic content; Decisioning for offer arbitration |
| Conflict / fatigue | Marketo communication limits only | Channel quiet hours + freq caps; Journey entry/concurrency + arbitration |
| Content supply | Marketo assets / legacy creative ops | GenStudio (GSPEM) → AJO |
| Measurement | Program reports | CJA: AJO + web + Marketo baselines |
| Stage handoff | Campaign flow / remove from stream | Exit criteria + new entry (do not Jump from Read Audience for clean handoff) |

---

## Choose platform

| Choose **Marketo** when | Choose **AJO B2C** when |
|---|---|
| CRM scoring, sales alerts, opportunity-stage heavy logic | Real-time activation / abandon / event-attended moments |
| Broad Discover / brand / topic streams at scale today | Email must sync with Target PZN on the same intent |
| Interim coverage while AJO pattern is in pulse-check | Overlapping patterns need priority / concurrency control |
| Need is merge-field personalization only | One shell + many offers beats cloning journeys per event/SKU |

---

## Hard coexistence fact

**AJO and Marketo frequency caps are not aware of each other.** Dual-runtime requires explicit excludes, audience partitioning, and clear pattern ownership — not “both platforms will figure it out.”

### Control matrix

| Control | Marketo | AJO B2C | Coexistence practice |
|---|---|---|---|
| Who can be messaged | Marketo marketable / list membership | `profile.b2b._redhat.marketable` + AEP audience | Same identity family (`b2b_person`); don’t double-enroll the same pattern |
| Send limits | Marketo comm limits | Channel freq caps + quiet hours (**promo**); exclude **transactional** by default | Cross-system excludes; document pattern owner |
| Journey concurrency | Engagement program rules | Journey entry / concurrency capping + arbitration | Align priority scores to Experience Patterns |
| Measurement | Program success | CJA AJO + web + Marketo views | Cite lifts only after 30/60/90 pulse checks |

### Exclude checklist (per migrated pattern)

1. Name the **pattern owner** (AJO or Marketo) for that product line + stage.
2. Suppress the losing platform’s overlapping smart campaign / stream / journey.
3. Confirm marketable + consent path on both sides before cutover.
4. Keep transactional Trial Start–class sends out of marketing quiet hours / promo caps.
5. Log the Marketo baseline rows used in CJA before claiming lift.

---

## AJO journey shapes (replace “one Marketo program type”)

Pick the type from the **entry signal**, not the email calendar.

| Comm class | Entry | Framework job | RH example |
|---|---|---|---|
| Promotional — scheduled nurture | Read Audience (daily / incremental) | Ladders | Learn Deep–style product interest / engagement |
| Promotional — streaming fallout | Audience Qualification | Nets | RHEL Trial Abandonment (if AQ qualifier) |
| Transactional | Unitary event | Catapults | RHEL Trial Start Welcome / Expiring / Survey |

**Handoff rule:** Jump is a **fork** (active in A+B), not a clean stage handoff. Scheduled nurture → Trial Start = **exit A + unitary-enter B**. See parking lot **PA-03** / **PA-04** / **PA-08**.

---

## Success bar (pilot pillars)

| Pillar | Intent |
|---|---|
| Customer experience | Seamless switch; no CX degradation vs Marketo sends |
| Performance | Hold or lift pattern KPIs vs YoY / Marketo baselines in CJA |
| Platform | GenStudio + AJO + CJA + Target work with Marketo still in the estate — without silent double-nurture |
