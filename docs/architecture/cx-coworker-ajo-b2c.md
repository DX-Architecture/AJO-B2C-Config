# CX Enterprise Coworker — AJO B2C use cases, prompts & custom skills

**Audience:** Digital Architecture (journey authors, measurement, Q1 cohort).  
**Role:** Practical Coworker runners for AJO B2C day-to-day work.  
**Not this doc:** Full-stack Coworker chapter (Marketo / Workfront / AJO B2B) stays in the handbook draft; dense contracts stay in **Handbook Ch2 / Ch6**. When this file and the handbook disagree on a contract, **handbook wins**.

**Related:** [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md) · [`../TRACKS.md`](../TRACKS.md) · [`journey-build-requirements.md`](journey-build-requirements.md) · [`../RHEL-Trial-Experience.md`](../RHEL-Trial-Experience.md)

**Last updated:** 2026-11-18

---

## 1. Ways of working (AJO B2C slice)

* Handbook remains the **contract book**. Coworker is a **runner** for repeatable checks, scaffolding, and CJA readouts.
* Prefer **Ask** (read/explain) over **Do** (create/change/publish) until verified in AJO / CJA UI.
* Use **Plan mode** for any Do prompt (journey create, content save, audience publish).
* Confirm **organization + Experience Platform sandbox** before asking.
* **Fail closed.** Missing data, unnamed nodes, or unverified joins → BLOCKED / inconclusive — never invent a pass.
* Spot-check load-bearing numbers in Workspace / AJO. Coworker narrative is not measurement SoT.
* After a good run: **Turn this into a reusable skill** named `[DA] …`, then put trigger phrases here.

### Helps vs does not replace

| Helps | Does not replace |
|---|---|
| Journey pre-flight (conflict, caps, node naming) | AEP event catalog authorship / CJA→AEP mapping |
| Fallout, anomaly, profile “why no send/offer?” | GenStudio Handlebars / brand-score field contracts |
| CJA pulse + empty-funnel triage | Inventing baselines or “shipped lift” claims |
| Draft journey from brief (human publishes) | Skipping track-2 build checklists |

---

## 2. AJO B2C use cases (OOTB skills)

| Use case | What Coworker does | When to use |
|---|---|---|
| **Create journey from brief / flowchart** | Orchestrates journey + message structure from plain language or uploaded image | New promo / transactional draft — leave inactive; human reviews track-2 settings |
| **Conflict / overlap** | Audience overlap, schedule collisions, same-channel window | Before live; Marketo Learn Deep coexistence |
| **Fallout analysis** | Where profiles drop; highest-fallout nodes | Hypercare / pulse when entries or progression look wrong |
| **Anomaly detection** | Spikes/drops/flatlines in entry, exit, send vs baseline | “Why did entries drop yesterday?” |
| **Journey hygiene / config audit** | Configuration issues + suggested fixes | Pre-publish alongside track-2 checklist |
| **Profile / execution trace** | Read-only path of a profile through a published journey | “Why didn’t this abandoner get the email?” |
| **Decisioning explain** | Eligibility, ranking, capping for why offer shown/not | Partner newsletter / offer slots |
| **Custom action errors** | Failures / error-rate spikes on third-party actions | When external actions are in the canvas |
| **CJA pulse / root cause / exec summary** | Metrics, funnels, stakeholder digest → open in Workspace | 30–60–90 pilot reads; weekly DA pulse |
| **AA ↔ CJA validation** | Reconcile AA suite vs CJA data view | Empty or mismatched delivery suspicion |

**Availability note:** AJO B2C full-page Coworker chat is Available; in-product chat may still be rolling. Confirm org enablement with Adobe TAM / admin.

---

## 3. Custom skill plan (stand up in order)

OOTB skills are Adobe playbooks. **Custom skills** encode *our* gates (Ch2 caps/conflict, Ch6 measurement, Marketo coexistence).

| Order | Skill name | Job | Trigger phrases |
|---|---|---|---|
| 1 | `[DA] AJO B2C Pre-Flight` | Read-only CLEARED / BLOCKED before go-live | `run AJO pre-flight`, `is this journey ready`, `AJO B2C pre-launch checklist` |
| 2 | `[DA] RHEL B2C CJA Pulse` | Fixed pulse vs Marketo baselines | `run RHEL B2C pulse`, `B2C pilot readout`, `CJA pulse for RHEL trial` |
| 3 | `[DA] CJA Empty Delivery Debug` | Ordered empty-funnel triage; never blame CSS first | `debug empty CJA`, `empty delivery for`, `why is the trial funnel empty` |

**How we create one**

1. Run once against a real journey / CJA view.  
2. Verify every load-bearing claim in the product UI.  
3. Ask Coworker: *Turn this into a reusable skill* named `[DA] …`.  
4. Parameterize dynamic dates on pulse skills.  
5. Publish for the team; keep trigger phrases in this file.  
6. When handbook Ch2/Ch6 change, update the skill — skill follows contract.

**Adoption:** Ship skills **1 + 2** first in a working session. Add skill **3** once the pulse is trusted.

---

## 4. Master prompts (paste into Coworker)

Replace bracketed values. Keep fail-closed language. After a verified run: *Save this as a skill called “[DA] …”*

### 4.1 `[DA] AJO B2C Pre-Flight`

```
Save this as a skill called “[DA] AJO B2C Pre-Flight.”

It is a read-only governance gate for Adobe Journey Optimizer B2C journeys before go-live.
It never publishes, activates, mutates, or copies anything.

Resolve the named journey from context or the user’s mention.
Evaluate the gates below against Digital Architecture handbook Chapter 2 (capping, conflict, suppression, measurement join to CJA) and AJO-B2C-Config journey-build requirements.
Render one scorecard:

- Alert banner with overall verdict
- One row per gate: Gate | Status | Finding | Required Fix
- Verdict: CLEARED only if every gate passes; BLOCKED if any fails
- Missing data = BLOCKED (fail closed). Never assume success.

Trigger phrases:
- “run AJO pre-flight”
- “is this journey ready”
- “AJO B2C pre-launch checklist”

Gates:

Gate 1 – Identity & journey type
Pass only if journey type matches the audience pattern (Read Audience vs Audience Qualification vs unitary) and the identity namespace required by the journey is stated (Persons promo ref: b2b_person). If identity or sandbox is unclear → BLOCKED.

Gate 2 – Conflict / overlap
Check overlapping start/end, audience overlap with other live or scheduled journeys, and same-channel collision in the same window (handbook 2.6). Surface any Marketo coexistence risk called out by the user (e.g. Learn Deep 90-day exclude). If conflict tools return nothing conclusive → BLOCKED with “inconclusive — verify in AJO View Potential Conflicts.”

Gate 3 – Caps & suppression
Confirm journey-domain rule set expectations for this stack where visible: concurrency (max 2 journeys), frequency (≤1 email/SMS per day Marketing), quiet hours / suppression awareness. RHEL Learn Deep 90-day exclude is a journey exclude, not a substitute for frequency. Channel rule set present for promo vs txn as required by journey-build-requirements. Missing cap evidence → BLOCKED.

Gate 4 – Node naming for CJA
Every send/action node that must appear in CJA measurement must be named. Unnamed send nodes → BLOCKED (handbook 2.7 / 6).

Gate 5 – Exit / event honesty
If the journey claims an event-based exit (e.g. trial started), do not treat a CJA segment definition as an AEP unitary event. Do not treat RHPT Start Click as activation. Flag catalog/streaming gaps as BLOCKED with owner = IT / AEP — not “fix in CJA and ship.”

For every BLOCKED gate, give the smallest human fix and which handbook / AJO-B2C-Config section applies.
```

**One-shot Ask:**

> Run AJO pre-flight on journey **[Product Trial Abandonment]** in sandbox **[prod/name]**. Include coexistence with Marketo Learn Deep. Confirm namespace, marketable gate, exit criteria, and Channel rule set.

---

### 4.2 `[DA] RHEL B2C CJA Pulse`

```
Save this as a skill called “[DA] RHEL B2C CJA Pulse.”

It produces a stakeholder-ready pulse for the RHEL AJO B2C pilot / trial-path experiences using Customer Journey Analytics.
It is read-only. It does not create segments or change Workspace projects unless the user explicitly switches to Do and approves Plan mode.

Methodology (fixed — do not invent alternate success definitions):
- Data view: Global | prod | user:email unless the user names another approved DA view
- Outer container: Person (not Session) for email → trial paths
- Channels in scope: AJO email (+ PZN if present), web activity, Marketo email activity
- Compare channel performance vs Marketo experience baselines when the user supplies them; otherwise state “baselines not provided”
- Content lens: GenStudio / GSPEM-authored AJO creative vs legacy Marketo creative when both exist in-range
- CTA / outcome lens: trial start engagement and RHPT Trial Activations (not “journey finished”)
- Path recipe reminder: Person → (named AJO/Marketo send landing on _redhat.maEmail* if present) THEN RHPT Trial Activations
- Do not use Eloqua unique email metrics on Marketo sends
- Do not use RHPT Success (60D Persistence) as a sequence step
- Anonymous SPA traffic without email will drop out of user:email — call that out before blaming creative

Dynamic dates: default last 30 days; accept last 7, 30–60–90, or explicit range from the user.

Output:
1. KPI strip (sends/deliveries, opens/CTR if available, trial activations)
2. What moved vs prior period (root-cause hypotheses, ranked)
3. Risks / data-quality flags (empty maEmail*, unnamed nodes, view mismatch)
4. 3 recommended next actions for DA (not generic Adobe tips)
5. Offer: “Open this in Analysis Workspace” / turn into slide outline on request

Trigger phrases:
- “run RHEL B2C pulse”
- “B2C pilot readout”
- “CJA pulse for RHEL trial”
```

**One-shot Ask:**

> Run RHEL B2C pulse for the last **30** days. Journeys in scope: **Product Trial Abandonment**, **Product Trial Start**. Marketo baselines: Learn Deep open 27.2% / CTR 6.0%; RHPT Trials open 39.0% / CTR 4.9%. Flag anything that looks like an empty-delivery or anonymous-trial issue.

---

### 4.3 `[DA] CJA Empty Delivery Debug`

```
Save this as a skill called “[DA] CJA Empty Delivery Debug.”

Read-only triage when an email → site or email → trial funnel looks empty in CJA.
Never blame Marketo CSS or GenStudio first.

Follow this order and stop with a clear owner when evidence is missing:

1. Confirm Workspace data view is Global | prod | user:email (or the named DA view) — not a Web-only view unless intentional.
2. Sanity table A: Email | Email Activity Type + Marketo | Unique Email Deliveries (or AJO feedback equivalent if analytics confirmed AJO lands elsewhere) + People.
3. Sanity table B (if trial path): RHPT Step Name (v120) + RHPT Trial Activations + People.
4. If A is empty → wrong view, send not in _redhat.maEmail*, or AJO send not covered by Marketo metric names. Owner: analytics / send-time ops. Not a CSS bug.
5. If B is empty → no successes in-window or SPA never attached email. Owner: product/analytics. Check login coverage before blaming email.
6. If A and B have rows but THEN path is empty → person ID stitch / date-range / anonymous trial. Owner: identity / analytics.
7. Check whether journey/send nodes were unnamed (AJO) — unnamed actions are not diagnosable.

Output a short incident card: Symptom | Evidence checked | Likely cause | Owner | Do not do (anti-patterns).

Trigger phrases:
- “debug empty CJA”
- “empty delivery for”
- “why is the trial funnel empty”
```

**One-shot Ask:**

> Debug empty CJA for **[program or journey name]** on **[date range]**. Path expected: Marketo/AJO delivery → RHPT Trial Activations.

---

## 5. Quick prompts (no skill required)

**Conflict spot-check (Ask):**

> Does **[journey]** conflict with any other live or scheduled journeys on audience, schedule, or email channel? Include known Marketo coexistence: Learn Deep 90-day membership.

**Fallout (Ask):**

> Where are people dropping off in **[Product Trial Abandonment]**? Which nodes have the highest fallout?

**Anomaly (Ask):**

> Why did entries / sends drop for **[journey]** in the last **7** days?

**Profile miss (Ask):**

> Why didn’t profile **[id]** enter or progress in **[journey]** on **[date]**? Trace identity, audience membership, time constraints, and caps. Do not change the journey.

**Decisioning miss (Ask):**

> Why did profile **[id]** see / not see offer **[X]** on **[date]**? Trace eligibility, ranking, capping. Do not change rules.

**Create draft journey (Do — Plan mode):**

> Create a draft **[Read Audience | unitary]** journey for **[goal]** from this brief / flowchart. Name every send node for CJA. Leave unpublished. Summarize settings for human review against journey-build-requirements (namespace, marketable, exit, Channel rule set).

**Hygiene audit (Ask):**

> Audit configuration hygiene for **[journey]**. List issues and the smallest fix for each. Do not publish.

---

## 6. Anti-patterns (AJO B2C)

* Treating CJA component names as AEP schema paths.
* Using Coworker exec summaries as quarterly “shipped lift” without a human result sentence.
* Assuming Marketo metric names cover AJO sends without analytics confirmation (handbook 2.7).
* Treating RHPT Start Click as activation / exit success.
* Saving a personal custom skill that diverges from Ch2 / this file — update the skill when contracts change.
* Running Do prompts with Plan mode off in prod sandboxes.
* Skipping track-2 build checklists because Coworker “built a journey.”

---

## 7. Canonical artifacts

| Artifact | Role |
|---|---|
| This file (`docs/architecture/cx-coworker-ajo-b2c.md`) | AJO B2C Coworker use cases, prompts, skill SoT in git |
| Handbook Ch2 / Ch6 | Contracts, failure modes, CJA paths |
| [`journey-build-requirements.md`](journey-build-requirements.md) | Promo vs transactional author checklist |
| [`../RHEL-Trial-Experience.md`](../RHEL-Trial-Experience.md) | Pilot narrative / scorecard |
| Experience League — Coworker Chat use cases / create first skill | Vendor how-to |
| Coworker → Customizations → Skills | Where `[DA]` skills live after publish |
