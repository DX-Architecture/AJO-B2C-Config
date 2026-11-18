# AJO B2C Pilot — Project Brief

**Program title (deck):** RHEL Learn Deep → Trial Start  
**Product:** Red Hat Enterprise Linux (RHEL) Server  
**Platform:** Adobe Journey Optimizer (AJO) B2C  
**Production launch:** October 14, 2026  
**Integrated stack:** GenStudio · AJO B2C · CJA  
**Status:** Launch-ready (v1 WIP deck)

---

## 1. Why this pilot

After ~11 months from kickoff to production readiness (advisory setup, identity schema, OOTB connector pivot, CJA/DNS alignment), we are cutting over the RHEL trial experience to **AJO B2C**.

The pilot proves we can move from native-platform execution to an orchestrated stack — **GenStudio creates content → AJO B2C activates the journey → CJA analyzes performance** — without degrading deliverability or trial outcomes.

Mapped to the **CY26 Marketing Scorecard:**

| Scorecard pillar | Pilot contribution |
|---|---|
| **Customer Success** | Proactive trial-start experience that reduces post-download drop-off |
| **Engagement** | Intelligent lifecycle orchestration replacing siloed journeys |
| **Efficiency** | GenStudio AI accelerating nurture content; CJA OOTB connection for analysis |
| **Growth and Demand** | Frictionless product trials that meet buyers digitally |

---

## 2. Experiences launching

**Arc:** Nurture → Trial Start

### Product Trial Abandonment [Email Nurture]

Re-engaging users who have shown interest in the RHEL Server trial but abandoned registration — giving them a path to learn more, with a late-stage offer that primes return to trial.

| | |
|---|---|
| **Entry signal** | User clicks “Start your trial” on the RHEL Server try-it page, then drops out of SSO without completing download |
| **Audience** | Real-Time CDP batch segment + exclusion rules |
| **Activation** | AJO B2C triggers email; Adobe Target can serve on-site trial offer (omnichannel) |
| **Creative** | GenStudio / GSPEM AI-enhanced content (SysAdmin + Architect persona tuning; AEM-grounded assets) |
| **Job** | Recover abandoners → educate → late-stage trial prime |

**Orchestration flow (deck):**

```text
Visitor lands (Start your trial CTA)
        → Abandonment (SSO drop before download)
        → AEP audience (batch segment + exclusions)
        → Re-engagement (AJO email + Target on-site trial offer)
```

### Product Trial Start [Transactional Email]

Engage and accelerate RHEL Server trial users in real time with transactional emails — **Welcome**, **Expiring Soon**, and **Survey** — to ensure successful trial activation.

| | |
|---|---|
| **Audience** | Users who have started the RHEL Server trial |
| **Channel** | Transactional email (event-triggered) |
| **Messages** | Welcome → Expiring Soon → Survey |
| **Job** | Reduce post-start drop-off; guide activation before expiry; capture feedback |

### How they work together

```text
Interest / try-it CTA
        │
        ├─ abandoned registration ──► Product Trial Abandonment (nurture)
        │                                    └─► return to trial path
        │
        └─ trial started ───────────► Product Trial Start (transactional)
                                             Welcome → Expiring Soon → Survey
```

---

## 3. Platform benefits (why AJO B2C)

From the deck workflow / orchestration story:

| Benefit | What we demonstrate |
|---|---|
| **Seamless platform switch** | Preserve email deliverability without customer-experience degradation |
| **Omnichannel cohesion** | Synchronize AJO email with Adobe Target on-site personalization |
| **Operational velocity** | Faster creative turnaround via GenStudio AI content supply chain |
| **Lifecycle orchestration** | Nurture + transactional trial moments in one B2C journey layer (vs siloed native flows) |
| **Interoperability** | Smooth operation with Marketo and Target while validating GSPEM + CJA |
| **In-platform learning** | CJA dashboards for AJO + web + Marketo in one measurement system |

**GenStudio content supply chain (pilot):** AEM asset grounding → full-stack copy automation (subject, headers, body, dynamic CTAs) → CDP persona tuning (SysAdmin / Architect).

---

## 4. Orchestrated experience: GenStudio → AJO B2C → CJA

```text
GenStudio — content development
        →  AJO B2C — journey activation
                →  CJA — journey analysis
```

| Layer | Role |
|---|---|
| **GenStudio (GSPEM)** | Brand-governed AI content for enhanced Product Trial Abandonment emails; speed + consistency vs prior Marketo creative |
| **AJO B2C** | Audience qualification, nurture + transactional triggers, journey logic; pairs with Target for on-site offer |
| **CJA** | Channel, content, and CTA performance vs Marketo / global benchmarks |

**Soundbite:** *GenStudio builds the message, AJO B2C delivers the moment, CJA proves the outcome.*

---

## 5. Success metrics

### Success pillars

| Pillar | Intent |
|---|---|
| **Customer experience** | Seamless switch to AJO B2C; measure AI-enhanced Product Trial Abandonment emails via GSPEM |
| **Maintain performance** | Maintain or increase overall trial performance across channels vs established YoY trends |
| **Platform functionality** | Validate AJO B2C across channels and Adobe integrations (GSPEM, CJA) while ensuring smooth operation with Marketo and Target |

### Measurement framework (CJA)

**How**
- CJA dashboard: AJO channel performance (email + PZN) · Web activity · Marketo email activity

**What**
- **Channel performance** — vs global averages and Marketo experience benchmarks
- **Content performance** — GenStudio emails vs original Marketo emails
- **CTA performance** — successful trial starts and offer engagement

### Marketo baselines (pre-launch benchmarks)

| Experience | Total sends | Delivery | Open | CTR | CTO | Unsub | Bounce |
|---|---:|---:|---:|---:|---:|---:|---:|
| RHEL Learn Deep | 20,805 | 98.30% | 27.20% | 6.00% | 21.90% | 0.20% | 1.67% |
| RHEL RHPT Trials | 108,128 | 99.10% | 39.00% | 4.90% | 11.80% | — | 0.90% |

> Cite post-launch lifts only after CJA pulse checks — these rows are benchmarks, not outcomes.

---

## 6. What’s next

### Near-term roadmap (from pilot deck)

| Horizon | Focus |
|---|---|
| **CY26Q4** | 30-60-90 day pulse checks |
| **CY26Q4** | Decision framework discovery |
| **CY27** | Use case development for decisioning |
| **Follow-on migration** | Migrate **RHEL Discover** and **Learn Mid** to AJO B2C |

### Platform expansion (DA AJO B2C Focus Areas)

Pilot uses a fraction of AJO B2C. WF objective: create space for DA to learn, research, and document scalable use cases — package requirements for IT when development is needed. *Not every feature needs a use case.*

**Q4 focus areas** (current Focus Areas deck)
- **Decision Capabilities** — custom catalog attributes · decision items · decision rules (**IT Discover**, kickoff 10/20; use case identified)
- **Dynamic content** — conditional rules in personalization + Email Designer variants (**Ideation**; use case **OCP / OpenShift new SKUs**)

Idea backlog (post-pilot platform analysis): [`platform-analysis-parking-lot.md`](platform-analysis-parking-lot.md) — PA-01…08 (OpenShift pre-purchase, exit criteria, Jump/entry, etc.; **PA-07 unused**).

**Separate track (not platform expansion parking lot):** LocSync ↔ AJO Multilingual — [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md).

**CY27Q1 (projects + features)**
- **Projects / UCs:** Partner newsletter · OpenShift SKUs · RHEL Learn Mid
- **Decision rules** → planned Q1 production (Partner; Lizzy)
- **Priority scores / ranking formulas** → Q1 discovery (Eric/Ashleigh ↔ DG framework/patterns)
- **Multilingual content** → Q1 discovery *(Focus Areas horizon; depth lives on LocSync separate track, not PA-\*)*

**CY27Q2–Q4 (directional)**
- Summit 2027 post-event · Conflict detection · Content optimization / experiment / conditional rules
- Journey Optimize · path experiments · targeting rules · conditions · **Event Modernization** (see below)

#### Event Modernization (CY27, GH)

Use case: personalize nurture content/offers by **which event a prospect attended** (e.g. KubeCon, Mobile World Congress, Summit) instead of one generic nurture track — a Decisioning pattern (journey = cadence, decisioning = which offer/content block wins, event data = who qualifies).

**Real dependencies (flagged in the source doc, not AI-generated):**
- Consistent, accurate **event-level data** at tier 1 / tier 2 / tier 3 granularity for actual-scale events
- Org bandwidth: creating the time/space for associates to prioritize this if it's where the business needs to pivot

⚠️ Still **gated by event-data quality** (tier 1/2/3 catalog / schemas) even though the current Focus Areas Q4 headline is Decisioning + dynamic content — sequence event catalog readiness before Summit 2027 / Event Modernization build (CY27Q2+).

Full requirements walkthrough: [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md) §5.4.

---

## 7. Open / confirm

| Item | Note |
|---|---|
| Specific late-stage Abandonment offer/CTA copy | Named in journey intent; confirm final offer package |
| First 30-day pulse check date | Target ~mid-Nov 2026 |
| Learn Deep naming | Deck title = Learn Deep → Trial Start; live journeys = Abandonment + Trial Start |

---

**Last updated:** 2026-11-11  
**Sources:**
- `WIP_CY26 AJO B2C _ RHEL Trial Experience.pdf` (v1 deck)
- `Digital Architecture - AJO B2C_Focus Areas.pdf` (refreshed 2026-11-11 — Q4 Decisioning + OCP dynamic content; CY27Q1 projects)
