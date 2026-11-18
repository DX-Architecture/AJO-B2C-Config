# Nets / Ladders / Catapults → AJO journey types

**Purpose:** Map Omnichannel Nurture Framework pillars to AJO B2C entry models and build patterns.  
**Sources:** [`../sources/cy25-nets-ladders-catapults.png`](../sources/cy25-nets-ladders-catapults.png) · [`../sources/cy25-experience-pattern-criteria.png`](../sources/cy25-experience-pattern-criteria.png) · [`../docs/architecture/journey-build-requirements.md`](../docs/architecture/journey-build-requirements.md)

---

## Framework intent (unchanged)

Every Experience Pattern should have strategic plans for:

| Pillar | Subtitle | Job |
|---|---|---|
| **Nets** | Fallout | Catch contacts, accounts, and users who left their journey |
| **Ladders** | Progression | Keep slow movers progressing through the journey |
| **Catapults** | Velocity | Accelerate based on specific behaviors and intent signals |

AJO changes the **mechanism**, not the intent.

---

## Pillar → AJO mechanism

| Pillar | Marketo today | AJO B2C mechanism |
|---|---|---|
| **Nets (Fallout)** | Re-engagement smart campaigns / separate nurture clones | AQ or unitary catch → recovery journey; Decisioning recovery offers + item/journey caps so the same net is not hammered |
| **Ladders (Progression)** | Wait steps + engagement gates; content hard-coded per stream | Read Audience nurture shell; dynamic content / Decisioning for next-best block; **Exit criteria** = framework release |
| **Catapults (Velocity)** | Behavioral triggers that still enqueue into email cadence | Unitary transactional + promo in one B2C layer (Trial Start pattern) |

---

## Entry model cheat sheet

| Pillar | Default AJO entry | Latency | Comm class | Channel rules |
|---|---|---|---|---|
| Ladder | **Read Audience** (daily + incremental) | Batch schedule | Promotional | Candidate for freq caps + quiet hours |
| Net | **Audience Qualification** (membership flip) | ~15–30 min Edge→Hub | Promotional (streaming) | Same promo rule set; watch double-fire with Marketo |
| Catapult | **Unitary event** | True real-time | Transactional | **Exclude** marketing quiet hours / promo caps by default |

Namespace practice (Persons promo / pilot txn): **`b2b_person`**. Marketable gate when required: `profile.b2b._redhat.marketable == true`.

---

## Live RH examples (RHEL pilot)

### Net — Product Trial Abandonment

```text
Visitor lands (Start your trial CTA)
  → Abandonment (SSO drop before download)
  → AEP audience (batch segment + exclusions) / AQ
  → Re-engagement (AJO email + Target on-site trial offer)
```

- Job: recover abandoners → educate → late-stage trial prime  
- Do not treat “Start click” as activation; exit when trial actually started

### Catapult — Product Trial Start (transactional)

```text
Unitary trial-start event
  → Welcome → Expiring Soon → Survey
```

- Job: reduce post-start drop-off; guide activation before expiry  
- Keep Quiet hours / marketing freq caps **off** transactional actions unless CX explicitly wants them

### Ladder — Learn Deep–style (scheduled nurture)

```text
Read Audience (Persons, daily incremental)
  → standing promotional cadence
  → Exit when purpose met (e.g. trial started) → unitary Catapult, do not Jump
```

---

## Experience Pattern criteria → AJO objects

| Pattern criterion | AJO / Decisioning object |
|---|---|
| Objective | Owned by the Experience Pattern (unchanged) |
| Audience (catch) | RT-CDP audience or decision rule eligibility |
| Conversion / KPIs (release) | Journey **Exit criteria** + CJA KPIs |
| Content mix, destinations, messaging | GenStudio assets; later Decision items + collections + placements |
| Partner motion / messaging | Custom Decisioning attrs (`partnerModule`, `channel`, `language`) |

**Anti-pattern:** Creating a new Experience Pattern (or cloned journey) per SKU or event when a Decisioning offer slot would scale the same shell.

---

## Framework rule: one pattern per product line

You should only be in **one stage and one Experience Pattern for a single product line** at a time (other product lines can differ).

Implication for AJO **Jump**: Jump is a **fork** (profile stays active in A and enters B). Dual-active messaging journeys on the same product line **violate** the framework unless A is deliberately non-messaging after Jump. Prefer **exit + new entry** for Learn → Trial Start handoffs. See parking lot **PA-03**.

---

## Decisioning (optional elevation inside a pillar)

Use Decisioning when context chooses **which offer/content block wins** inside a Net, Ladder, or Catapult shell.

| Need | Tool |
|---|---|
| Insert `{{eventName}}` only | Profile/event personalization — no Decisioning |
| Bounded set of offers by event/persona/partner | Decisioning catalog + policy |
| Huge product catalog recommendations | Target Recommendations (not offer Decisioning) |
| Which *journey* wins when many qualify | Journey capping / arbitration (not item Decisioning) |

Depth: [`../docs/architecture/decisioning-items-offers-requirements.md`](../docs/architecture/decisioning-items-offers-requirements.md)
