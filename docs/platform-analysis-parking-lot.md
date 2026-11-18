# AJO B2C — Platform analysis parking lot

**Track:** 1 — Platform expansion · index [`TRACKS.md`](TRACKS.md)

**Purpose:** Capture ideas to explore, do, or include in a deeper AJO B2C platform analysis **after the RHEL trial pilot has more runtime**. Not a full analysis yet — a structured backlog so thoughts don’t get lost.

**Trigger to graduate this into a full analysis:** enough post-launch signal from CJA pulse checks + hypercare to separate “pilot gaps” from “platform expansion bets.” Until then, park ideas here and map them to DA Focus Areas.

**Related**
- Two-track home: [`TRACKS.md`](TRACKS.md)
- Journey **build** settings (promo vs transactional) — track 2, not here: [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md)
- Focus Areas source (current): `Digital Architecture - AJO B2C_Focus Areas.pdf` · extract `sources/ajo-b2c-focus-areas-extract.txt` (refreshed 2026-11-11 from Downloads)
- Pilot brief / what’s next: [`project-brief.md`](project-brief.md)
- Decisioning deep dive (side track): [`architecture/decisioning-items-offers-requirements.md`](architecture/decisioning-items-offers-requirements.md)
- LocSync / Multilingual (side track): [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md)
- Q4 team track: `../Professional/q4-focus-areas.md` → AJO B2C platform track

---

## How to use

1. Drop ideas into **Backlog** with a Focus Area tag.
2. When you’re ready to research one, move it to **In exploration** and fill the exploration card.
3. Outcomes become either a Focus Areas use-case write-up (platform template) or a “won’t pursue / later” note — don’t leave findings only in chat.

**Status legend:** `idea` · `explore` · `recommend` · `defer` · `done`

---

## Backlog

| ID | Idea | Focus Area (DA) | Status | Why it matters | Notes / links |
|---|---|---|---|---|---|
| PA-01 | Weekday-only email sends (custom waits) | Activities / orchestration patterns *(adjacent to Conflict mgmt timing)* | `idea` | Professional / B2B-like send windows; weekend entries shouldn’t fire immediately | [Adobe: Send emails only on weekdays](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/journey-use-cases/weekday-email-uc) |
| PA-02 | Quiet hours vs frequency capping in the same Channel business rule set | Conflict management & prioritization | `idea` | Need a clear RH rule-set model for email limits + send windows before more journeys migrate | See exploration card below; Adobe Channel rule sets support both |
| PA-03 | Jump action — cleaned against dual-activity + nurture framework | Activities / orchestration · Conflict mgmt (concurrency) | `explore` | Jump = fork (active in A+B), not a clean handoff — redesign use cases vs one-experience-per-product-line | [Adobe: Jump](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/jump) |
| PA-04 | Journey entry model for scheduled email nurture (vs Jump) | Activities / events · Orchestration patterns | `explore` | Classic nurture = marketable + day/time + AEP membership → Read Audience; that path cannot use Jump — need a split model for relevance + lifecycle | See card; [Journey types](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/journey-types-selection) |
| PA-05 | AJO personalization (editor, profile data, playground, inline edit) | Content management · Dynamic content | `idea` | Bookmark for next build — profile tokens, Handlebars, playground practice before OpenShift SKU content | [Get started with personalization](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/personalization/personalize) |
| PA-06 | OpenShift Pre-purchase new build (CY27 SKUs / TM ideas) | Content mgmt · Decisioning · Experience Patterns | `idea` | New SKU map + modular email sections + Discover→Learn→Expand pre-purchase; personalization + decisioning opportunity | Source: `sources/tm-ideas-new-openshift-skus.pdf` |
| PA-08 | Journey **Exit criteria** (Event / Audience / Profile attribute) | Activities / orchestration · Experience Pattern “release” | `recommend` | **Also a track-2 build requirement** — stop nurture when purpose is met; pairs with PA-03/04 | Build: [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md) §4 · [Adobe Exit criteria](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/create-journey/journey-properties) |
| OPS-01 | Q1 permissions + user onboarding + best-practices pack | Tool process / docs / enablement | `idea` | Who gets AJO access for Q1 UCs; co-authored BPs (marketable, entry, namespace, unitary events, support) | [`ops/ajo-permissions-user-onboarding-q1-sketch.md`](ops/ajo-permissions-user-onboarding-q1-sketch.md) |
| OPS-02 | AEP schemas / fields for email personalization inventory | Content / data · Personalization | `idea` | Living list of verified `{{profile…}}` paths + known needs (marketable, persona, product…) | [`architecture/aep-email-personalization-fields.md`](architecture/aep-email-personalization-fields.md) |
| OPS-03 | Tag structure AEP/CDP/AJO B2C | Tool process / governance | `idea` | CDP audience naming vs AJO Unified Tag categories (milestone names, filterable metadata) | [`ops/aep-cdp-ajo-b2c-tag-structure-sketch.md`](ops/aep-cdp-ajo-b2c-tag-structure-sketch.md) |

*PA-07 retired — LocSync / Multilingual is a **side track**: [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md). Keep IDs stable (`PA-nn` / `OPS-nn`).*

*Journey **build** checklists (promo vs transactional settings) live in track 2: [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md) — not this parking lot.*

---

## Exploration cards

### PA-01 — Weekday-only email sends (custom waits)

**Question to answer later:** For RH AJO B2C (esp. Evaluate / trial / professional audiences), should weekend suppression be a **journey pattern** (condition + custom Wait) or a **channel Quiet hours** rule (Sat/Sun all-day)?

**Adobe pattern (journey-level)**
- Condition (time / day of week) → Saturday / Sunday / weekday paths
- Weekend paths: Wait with custom formula, e.g. Monday 9 AM  
  - Sat: `toDateTimeOnly(setHours(nowWithDelta(2, "days"), 9))`  
  - Sun: `toDateTimeOnly(setHours(nowWithDelta(1, "days"), 9))`
- Weekday path: send immediately
- **Timezone:** day-of-week evaluation uses the **journey’s** configured timezone, not the recipient’s

**When to prefer this pattern**
- Need Monday delivery at a **specific hour** after weekend entry
- Logic is journey-specific (only some journeys care about weekdays)
- Want explicit canvas visibility for marketers

**When to prefer Quiet hours instead (see PA-02)**
- Want reusable, channel-wide Sat/Sun (or nights) suppression across many journeys/campaigns
- Prefer Queue vs Discard policy centrally rather than per-journey Wait branches
- Willing to manage timezone via rule (fixed TZ or profile local TZ — with “no TZ on profile ⇒ not enforced” guardrail)

**Pilot relevance (parked)**
- Trial Start transactional (Welcome / Expiring Soon / Survey): weekend delay may or may not be desirable — confirm with CX / deliverability intent before applying broadly
- Abandonment nurture: more likely a weekday-window candidate if RH wants “workweek” engagement

**Explore later**
- [ ] Compare engagement if weekend sends are queued to Monday vs discarded
- [ ] Confirm journey TZ standard for RH sandboxes
- [ ] Document reusable journey snippet / pattern if we keep the custom-wait approach
- [ ] Decide: journey pattern vs Quiet hours (or both — Quiet hours for nights + journey Wait only where Monday-at-9 is required)

**Doc:** [Send emails only on weekdays](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/journey-use-cases/weekday-email-uc)

---

### PA-02 — Quiet hours inside the same business rule as email communication limits?

**Short answer (Adobe product model):** **Yes — they belong together in a Channel-domain rule set.** Frequency capping and Quiet hours are both Channel-domain rule types. Adobe’s rule-set design is explicitly: group them, activate the set, apply to journey/campaign email actions.

**Do not confuse domains**

| Domain | Rule types | Applied to |
|---|---|---|
| **Channel** | Frequency capping · **Quiet hours** | Channel actions (email, etc.) in journeys/campaigns |
| **Journey** | Entry capping · Concurrency capping (+ arbitration) | Journey properties (capping rules) |

So: quiet hours + email send limits = **same Channel rule set** (recommended for a coherent “email governance” package).  
Journey capping / arbitration = **separate Journey rule set** (already on the Q4 Focus Areas list).

**Important product constraints to validate in our sandbox**
- Quiet hours live in **custom** Channel rule sets — **not** the Global Default Rule Set
- Category for Quiet hours is currently Marketing-oriented (confirm transactional email behavior / exclusions for Trial Start)
- Handling during quiet hours: **Queue** (send when window ends; discard if queued > 7 days) vs **Discard** (profile exits that message / journey path — high impact)
- Quiet hours updates can take up to ~12 hours to propagate to actions already using the rule
- Profile local TZ option: if profile has **no** timezone, quiet hours are **not** enforced for that profile

**Decision framing for RH (for the full analysis)**

1. **One Channel rule set for “marketing email governance”?**  
   e.g. frequency cap(s) + quiet hours (nights + weekends) applied to nurture / promotional journeys.
2. **Separate rule sets by communication type?**  
   e.g. Promotional vs Transactional — especially if Trial Start must never wait for quiet hours.
3. **Global frequency vs custom sets?**  
   Global Default for org-wide floor; custom sets for journey-family overrides (Evaluate vs lifecycle vs events).
4. **Quiet hours vs weekday custom waits (PA-01)?**  
   Prefer Quiet hours for reusable windows; keep custom Wait only when we need a precise “fire Monday at H” merge into one send step.

**Explore later**
- [ ] Inventory current business rules in the B2C sandbox (global + custom)
- [ ] Confirm which pilot journeys (if any) already have a Channel rule set on the email action
- [ ] Propose RH standard: Marketing Email rule set contents (caps + quiet hours) + Transactional exception policy
- [ ] Align with Marketo coexistence: AJO and Marketo caps are **not** aware of each other (Focus Areas note) — excludes still matter
- [ ] Write Focus Areas use-case doc once recommendation is clear

**Docs**
- [Work with rule sets](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/conflict-prioritization/capping-rules/rule-sets)
- [Set quiet hours](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/conflict-prioritization/capping-rules/quiet-hours)
- [Capping rules landing](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/conflict-prioritization/capping-rules/capping-rules-landing-page)

---

### PA-03 — Jump action (cleaned): dual-activity × Omnichannel Nurture Framework

**Status:** `explore` — ideas pruned against Adobe dual-activity behavior + framework stage rule.  
**Doc:** [Jumping from one journey to another](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/jump)

#### Hard facts (design from these first)

1. **Jump is a fork, not a handoff.** When the profile hits Jump in Journey A, they **keep progressing in A** and **also** enter Journey B. They are **active in both at the same time**.
2. **Framework rule:** one stage / one Experience Pattern **per product line** at a time (other product lines can differ). Dual-active messaging journeys on the same product line **violate that rule** unless A is deliberately non-messaging after Jump.
3. **Entry constraints:** Jump only between compatible **event-based** shapes (same namespace). No Jump with / to **Read Audience**. AQ→event Jump is progressive rollout only. Scheduled marketable nurture stays **Read Audience** (see **PA-04**) — Jump is not that shell’s tool.
4. **Silent skip:** if already in B, Jump does nothing to B; A continues. No error.
5. **Payload contract:** Jump must populate B’s **first event** fields (RHEL Trial Start example below).

```text
Journey A (origin)                         Journey B (target)
  … → Jump ──internal event──► first event → … (B runs)
         │
         └─► A continues (waits, emails, End…)   ← still active
```

#### RH design rule (proposed)

| Intent | Allowed Jump shape | Origin after Jump | Why |
|---|---|---|---|
| **Release → next pattern** (ladder / stage change) | **Terminate-style:** Jump then **End** (or only non-send closeout) | No further emails/pushes on A | Approximates handoff; minimizes dual-experience messaging |
| **Spawn parallel module** (rare) | Jump into a short non-competing module | A may continue only if B is non-overlapping channel/purpose | Explicit exception; needs concurrency cap + freq rules |
| **Pattern change while A still nurtures** | **Do not Jump** | — | Dual active = two experiences on one product line |
| **Scheduled nurture → Trial Start** | **Do not Jump** from Read Audience | Exit A on unitary trial-start; B entered by that event | Matches pilot; avoids fork + entry incompatibility |

**Concurrency:** Journey-domain capping must assume Jump **increments concurrent journey count** (A+B). Cap of 1 effectively blocks useful Jump unless A ends almost immediately.

#### Framework drill-in: nets / ladders / catapults under dual-activity

| Pillar | Framework meaning | Jump? | Cleaned guidance |
|---|---|---|---|
| **Ladder (progression)** | Move to next stage/pattern when release KPI met | Only as **terminate-style** Jump A→B | A’s job is done → Jump → End A. B becomes the sole active experience for that product line. Prefer this over “Jump and keep nurturing in A.” |
| **Net (fallout)** | Catch people who fell out | Prefer **own Read Audience / AQ shell** for the net, or Decisioning recovery offers **inside** the current pattern | Jumping into a net **while** the parent pattern still emails = two nets/ladders at once. Jump **out** of a net into the next pattern only with terminate-style. |
| **Catapult (velocity)** | Accelerate on intent | Prefer **Decisioning / path conditions inside one journey**, or exit + unitary into a fast-path journey | Jump to a catapult journey **while** origin cadence continues = worst dual-messaging case. If Jump, terminate A immediately. |
| **Experience Pattern criteria** | Objective / catch / release / content mix | Jump does **not** replace catch/release | Catch = B’s entry event (or A’s before Jump). Release on A should be the Jump gate. Content mix = Decisioning inside A or B — not Jump. |
| **One product line / one experience** | Hard RH rule | Dual-active A+B is a **controlled exception window** | Keep the overlap window tiny (Jump→End) or forbid Jump for that edge. |
| **Decisioning** | Offer/content inside a pattern | Complement | Jump = which journey shell; Decisioning = which offer inside the active shell ([decisioning note §7](architecture/decisioning-items-offers-requirements.md)) |

#### Use cases — keep / reshape / drop

| # | Idea | Verdict | Why (dual-activity + framework) |
|---|---|---|---|
| 1 | Abandonment → Trial Start via Jump | **Drop as primary** | Trial Start already has unitary entry; Jump would fork Abandonment+Trial Start (Welcome + abandon emails). Use **event entry + exit Abandonment**; Jump only if Abandonment is event-based **and** terminate-style **and** dual-entry policy is explicit. Also: Read Audience origin can’t Jump (**PA-04**). |
| 2 | Bite-size phases inside one pattern (educate → prime → last touch) via Jump | **Reshape** | Only if each phase Jump is **terminate-style** (phase A Ends after Jump to phase B). Same pattern family, still dual-active briefly — treat as sub-journey ops, not two framework experiences. Alternative: one journey with waits (simpler; no concurrency hit). |
| 3 | Shared net module many parents Jump into | **Reshape / defer** | Parents must terminate or stop messaging; else parent + net both active. Often cleaner: net = its own audience entry, not Jump-fed. |
| 4 | Catapult fast-path via Jump while nurture continues | **Drop** | Explicit dual experience. Use in-journey branch or exit→unitary fast path. |
| 5 | Discover / Learn Mid → Evaluate via Jump | **Keep (terminate-style only)** | Good ladder story for migration — event-based Learn journey Jumps to Evaluate event journey then Ends. Scheduled Learn shells stay Read Audience without Jump (**PA-04**). |
| 6 | Post-event net → product evaluation via Jump | **Keep (terminate-style only)** | Same as ladder; gated on event catalog quality. |
| 7 | Shared survey / closeout pack via Jump | **Keep as spawn exception** | Origin may still be in Trial Start while Survey module runs — only OK if Survey is the intentional next step and origin doesn’t also send survey; prefer Jump→End on origin email path or make survey the last steps of one journey. |

#### Recommended RH patterns (canonical)

**A. Terminate-style ladder (framework-safe Jump)**

```text
Pattern / phase A (event-based)
  … release condition …
  → Jump to Pattern / phase B
  → End                         ← no more sends on A
Pattern B runs alone for that product line
```

**B. Scheduled nurture shell (no Jump)** — marketable + day/time + AEP membership

```text
Read Audience Evaluate shell
  → Decisioning (product/topic)
  → emails / waits
  → exit when unitary trial-started (or release KPI)
Trial Start journey enters from that unitary event alone
```

**C. Spawn (exception)** — only with written concurrency + channel rules

```text
Journey A → Jump to short module B → A continues without overlapping sends
```

#### RHEL Trial Start — Action parameters (payload contract)

Jump into Trial Start must map the target’s first-event fields (all required in current build):

| Parameter | Role |
|---|---|
| Created Date · Timestamp | Event timing |
| Offer Type · Campaign Name · Offer ID | Offer/campaign-shaped schema Trial Start expects |
| Email Address | On-event address (+ auto identity) |

Timeout: **Custom action / data source timeout (seconds)**.  
Constants are possible but secondary to real trial-start event values; confirm whether journey logic branches on Offer Type / Offer ID.

#### Explore later
- [ ] Adopt **terminate-style** as the only default Jump pattern in DA standards
- [ ] Set Journey concurrency expectation (e.g. cap ≥ 2 only if Jump→End is fast; else design for cap = 1 and no Jump)
- [ ] Confirm sandbox: which live journeys are event-based (Jump-eligible) vs Read Audience
- [ ] Dual-entry policy: Trial Start = unitary only (recommended) vs unitary + Jump
- [ ] Canonical constants if any Jump into Trial Start is ever allowed
- [ ] Migration blueprints: Learn Mid / Discover → Evaluate using terminate-style Jump vs audience exit + Evaluate Read Audience
- [ ] Focus Areas use-case draft only after standards above are agreed

---


### PA-04 — Audience / entry options for scheduled email nurture (and how relevance + Jump can still work)

**Your stated nurture shape:** marketable contacts · enter at a chosen day/time · based on AEP audience membership · send the most relevant product/topic.

That shape is **Read Audience** by design. Jump is **not** available on that journey (and that journey cannot be a Jump *target*). So “seamless” means a **split operating model**, not forcing Jump onto the scheduler.

#### The four entry options (what each is for)

| Entry | Who enters | When | Audience type | Jump? | Fits classic email nurture? |
|---|---|---|---|---|---|
| **Read Audience** | Everyone currently in the AEP audience (batch pull) | Schedule you set (once or recurring); optional “trigger after batch audience evaluation”; **incremental read** = only net-new since last run | Batch audiences preferred; also compositions / CSV | **No** (cannot contain Jump; cannot be Jump target) | **Yes — primary fit** for marketable ∩ stage on Tue/Thu 10:00 |
| **Audience Qualification** | One profile at a time when they **enter or exit** the audience | As membership changes (near real-time if **streaming** audience; batch AQ deprecated / blocked for publish from Aug 2026) | **Streaming** audiences only (for real-time intent) | **No** as start/target in general; progressive rollout: AQ-*origin* may Jump *to* an **event-based** journey (confirm sandbox) | Partial — evergreen “when you qualify” nurtures, not “everyone at 10:00” |
| **Unitary event** | One profile when *they* do something (abandon, trial start, form, intent signal) | Immediate | Event schema + identity (not “audience membership” as the start) | **Yes** — best Jump origin/target | Behavioral / transactional / catapult; not the weekly blast |
| **Business event** | Many profiles after a business-level signal; AJO then runs an internal **Read Audience** step | When the business event fires (+ audience export lag) | Business event + audience of who should hear it | Matrix lists Jump ✅; Jump doc also says no Jump + Read Audience in same journey — **validate in sandbox** before relying on it | Flash/sale/stock-style; weak fit for standing nurture cadence |

**Docs:** [Journey types](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/journey-types-selection) · [Read Audience](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/read-audience) · [Audience Qualification events](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/audience-qualification-events) · [Jump](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/about-journey-building/jump)

#### How to keep nurture “day/time + marketable + AEP” *and* stay coherent with Jump / patterns

Split **three jobs** that Marketo often mashed into one program:

```text
1) WHO + WHEN     → Read Audience journey (scheduler / Experience Pattern shell)
2) WHAT (relevance) → Decisioning or conditions INSIDE that shell (product / topic)
3) NEXT LIFECYCLE → Unitary-event journeys (+ Jump between those), not Jump from the Read Audience shell
```

```text
AEP audience (batch)
  marketable ∩ consent ∩ stage/pattern excludes
           │
           ▼  schedule (e.g. Tue 10:00) + optional post-segmentation trigger
┌──────────────────────────────────────────┐
│ Read Audience nurture (NO Jump)          │
│  conditions (still marketable, not sold) │
│  Decisioning / condition:                │
│    most relevant product OR topic        │
│  Email(s) + waits                        │
│  Listen / exit on unitary “trial started”│
└──────────────────────────────────────────┘
           │  (lifecycle handoff — event, not Jump)
           ▼
┌──────────────────────────────────────────┐
│ Unitary Trial Start journey (Jump OK)    │
│  Welcome → Expiring → Survey             │
│  optional Jump → shared closeout module  │
└──────────────────────────────────────────┘
```

**Product / topic relevance (do this inside the Read Audience shell)**  
Do **not** Jump to “RHEL journey” vs “Ansible journey” from a scheduled nurture — Jump isn’t available there, and one-journey-per-product explodes concurrency vs the framework’s “one stage/experience per product line.”

Prefer:

| Approach | When | How relevance works |
|---|---|---|
| **One pattern journey + Decisioning** | Multi-product or multi-topic interests in one Evaluate/Learn shell | Eligibility + ranking on profile attributes / affinities / last intent → winning email offer or content fragment |
| **Condition branch on profile attributes** | Few hard splits (e.g. RHEL vs OpenShift only) | Condition → different email actions; still one journey |
| **Separate Read Audience journeys per product line** | Strict product-line stage ownership | Audience definition already encodes product; journey stays simple — still no Jump between them |
| **Streaming AQ “catch” → event modules** | True always-on entry when someone newly qualifies | AQ (or unitary) enters a Jump-capable path; use Waits for send-window (PA-01) if you must approximate dayparting |

**If you need Jump for nets/ladders/catapults:** only on **event-based** modules, and default to **terminate-style** Jump (Jump → End on origin) so dual-activity does not leave two messaging experiences on one product line (**PA-03**). Scheduled nurture still *releases* via **behavior events** (trial start, etc.) — same as the pilot Abandonment shell + Trial Start event pairing.

#### Decision guide (RH nurture)

| Need | Choose |
|---|---|
| Marketable list, fixed day/time, AEP membership snapshot | **Read Audience** + schedule type **Daily** + **incremental read** (see [`ops/promotional-read-audience-settings.md`](ops/promotional-read-audience-settings.md); handbook §2.5.2 — RHEL Learn Deep reference) |
| Most relevant product/topic at send | **Decisioning / conditions inside** that journey — not Jump |
| Handoff to Trial Start / shared survey / next pattern phase with Jump | **Unitary-event** (or sandbox-confirmed AQ→event Jump) journeys |
| Enter the moment they qualify (evergreen), not on a clock | **Audience Qualification** on a **streaming** audience; then Wait to quiet hours / weekday if needed |
| React to a business moment affecting many people | **Business event** (+ audience); treat Jump as TBD until sandbox proves it |

#### Pilot / framework notes
- Current RHEL Abandonment = RT-CDP **batch segment + exclusions** → aligns with **Read Audience**, not Jump origin
- Trial Start = transactional **unitary** → Jump-compatible module; keep handoff event-driven
- Framework “one experience per product line”: encode product in **audience and/or Decisioning attributes**, not in Jump fan-out from a batch parent

**Explore later**
- [ ] Document RH standard: “Scheduled nurture = Read Audience; lifecycle modules = unitary (+ Jump)”
- [ ] Inventory which live/planned nurtures are truly schedule-based vs qualify-as-you-go
- [ ] Decisioning attributes for product + topic relevance on Evaluate shells
- [ ] Confirm AQ→event Jump rollout and Business-event + Jump behavior in our sandbox
- [ ] Incremental read + “trigger after batch audience evaluation” for marketable snapshot freshness

---

### PA-05 — Bookmark: Get started with personalization (AJO)

**Why parked:** Needed for the upcoming OpenShift / modular-email build (**PA-06**), and maps to DA Q4 **Content management → dynamic content**.

**Adobe page (bookmark):** [Get started with personalization](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/personalization/personalize)  
*On-page summary:* personalization editor · profile data you can use · learning playground · inline editing.

**What it covers (for build planning)**
- Handlebars-style tokens: `{{profile…}}` (e.g. name, interests, location, purchases)
- **Personalization editor** — select / arrange / validate expressions; helpers + predefined expressions
- **Data:** XDM Individual Profile schema (only schema for content personalization); optional **computed attributes**; dataset lookup personalization (beta)
- **Playground** — practice with sample payloads, no live datasets ([tutorial](https://experienceleague.adobe.com/en/docs/journey-optimizer-learn/tutorials/personalize-content/personalization-editor-playground))
- **Inline edit** — type `{{` in Email Designer / Push to autocomplete profile attributes without opening full editor
- Related: [Generate personalization expressions](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/generate-content/generative-personalization-expressions) (AI assist in editor)

**Split of duties (don’t conflate)**
| Need | Use |
|---|---|
| Insert profile fields / simple conditional copy | **Personalization** (PA-05) |
| Pick winning offer/block among catalog items | **Decisioning** (architecture note) |
| Which journey shell / stage | Entry model + Jump rules (PA-03/04) |

**Explore later**
- [ ] Walk playground before OpenShift email1 build
- [ ] List profile attrs required for OpenShift pre-purchase (persona, product+version, OpenShift intent, digital footprint) → fill [`architecture/aep-email-personalization-fields.md`](architecture/aep-email-personalization-fields.md)
- [ ] Decide what is personalization token vs decisioning content block in the 4-section template
- [ ] Verify XDM paths in AEP / Personalization editor (do not invent from CJA)

---

### PA-06 — New build opportunity: OpenShift Pre-purchase (TM ideas / CY27 SKUs)

**Source (in binder):** [`sources/tm-ideas-new-openshift-skus.pdf`](sources/tm-ideas-new-openshift-skus.pdf) (+ preview/OCR under `sources/`)  
**Origin:** `/Users/gdanish/Downloads/TM ideas for new OpenShift SKUs.pdf`

**What the board is saying (extracted)**
1. **SKU remap (legacy → updated)** — “AI generated from → to” / CURRENT LEGACY OFFERINGS kept for renewals vs updated CY27 stack, including roughly:
   - **OVE** — OpenShift Virtualization Engine (VM-only / KubeVirt scope)
   - **OpenShift Foundation (OF)** — foundation / Kubernetes platform motion (ACM + GitOps + Service Mesh called out on board)
   - **OpenShift Application Platform (OAP)** — app platform / DevSecOps motion
   - **OpenShift Security Suite (OSS)** — security add-on (ACS etc.)
2. **Legacy program rails still named:** Discover App Platform · Learn OpenShift · Evaluate OpenShift · Onboard · Adopt · Adopt OPP · Renew — map carefully to new SKU language without breaking renewals.
3. **Journey = OpenShift Pre-purchase** staged **DISCOVER → LEARN → EXPAND** (framework-aligned; not full Discover→Expand of every RH stage—this board’s pre-purchase slice).
4. **Intent > OpenShift** branch: **Upsell** (e.g. existing RHEL) vs **Standalone** paths — same modular email shell, different entry story.
5. **New email templates** — multi-section placeholders (`{{section1_headline}}`, body, CTA, 300×300 / 1200×600 slots) — built for dynamic fill.
6. **Email breakdown (Email 1 example):** existing **RHEL customer**, **developer persona**, **OpenShift intent**  
   - §1 Intent / hero — dynamic by persona — data: product + version  
   - §2 Product overview — dynamic by persona — product + version  
   - §3 Offer + value prop — **Decisioning** — product + version, digital footprint  
   - §4 Next best offer / secondary CTA — **Decisioning** — product + version, digital footprint  
   - Sample hero: “Code Faster on the Linux Foundation You Already Trust” → OpenShift Application Platform story; secondary “Elevate Your RHEL Apps to Enterprise Kubernetes”
7. **Orchestration sketch:** Discover email → wait (~3 days) → **Reaction** (click / high-value) vs no-reaction → next Discover or Learn module.

**How this hooks the parking lot**
| Capability | Role in this build |
|---|---|
| **PA-05 Personalization** | Persona greeting, product+version, lightweight dynamic lines in §1–2 |
| **Decisioning** | §3–4 offer / NBO blocks as catalog items by SKU (OVE/OF/OAP/OSS) + footprint |
| **PA-04 Entry** | Pre-purchase cadence likely **Read Audience** (marketable ∩ OpenShift intent / RHEL installed); Jump not on that shell |
| **PA-03 Jump** | Only if event-based Learn/Expand modules need terminate-style handoff later — not required for v1 template proof |
| **Nurture framework** | Experience Pattern = OpenShift Pre-purchase (Evaluate-leaning); Upsell vs Standalone = catch variants; nets/ladders/catapults TBD after SKU audience rules |

**Build framing (when ready)**
- Treat as first **non-RHEL-trial** AJO B2C pattern candidate after pilot learning — proves modular content + SKU taxonomy.
- Do **not** invent live metrics; board is ideation (templates + SKU map + example copy).

**Explore later**
- [ ] Confirm official CY27 SKU names/scope with product marketing (OCR/board names are approximate)
- [ ] Audience defs: RHEL upsell vs OpenShift standalone; marketable; excludes
- [ ] Profile attribute contract for persona + product/version + OpenShift intent + digital footprint
- [ ] Section template ownership: GenStudio vs AJO fragments vs Decisioning items
- [ ] Focus Areas use-case write-up once build is greenlit

---

### PA-08 — Journey Exit criteria (Event / Audience / Profile attribute)

**Status:** `recommend` — author contract lives in track 2 [`ops/journey-build-requirements.md`](ops/journey-build-requirements.md) **§4**. This card keeps expansion / pattern examples.

**Why:** Direct OOTB match to Experience Pattern **release** (conversion / “stop messaging when purpose is met”). Complements entry model (**PA-04**) and avoids over-relying on Jump (**PA-03**) for handoffs.

**UI (sandbox):** Journey canvas → Show Exit criteria → Add exit criteria → Label + type:
- **Event** (unitary only) — e.g. trial started, purchase  
- **Audience** — qualifies for “converted / purchased” audience (can take ~10 min to be effective)  
- **Profile attribute** — special case: **paused journeys only**, one rule per journey, evaluated at **action** steps  

Multiple Event/Audience criteria = **OR**, evaluated at every step. Configured in draft. Stops further communications from that journey when met.

**Docs:** [Journey properties — Exit criteria](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/create-journey/journey-properties) · [Entry & exit guide](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/journey-use-cases/entry-exit-criteria-guide)

**Framework / RH examples**
| Pattern | Exit idea |
|---|---|
| RHEL Abandonment | Unitary **trial started** (Focus Areas omnichannel example already: exit on started trial) |
| OpenShift Pre-purchase | Trial start / intent-fulfilled / product commit events or audiences |
| Partner newsletter | Less about exit; more Decisioning — still use exit if “unsubscribed / left partner program” audience |

**Vs Jump:** Exit = leave *this* journey when success/irrelevant. Jump = fork into another journey (dual-active). Prefer **exit + separate unitary entry** for Trial Start over Jump from nurture.

**Explore later**
- [ ] Confirm exit criteria on live Abandonment / Trial Start journeys
- [ ] Standard: every Experience Pattern documents catch + **exit/release** in the DA template
- [ ] Audience-based exits: streaming vs batch lag expectations

---

## Focus Areas alignment (current deck, 2026-11-11)

Source: updated `Digital Architecture - AJO B2C_Focus Areas.pdf`

| Horizon | Official Focus Areas | Parking-lot / wrap-up alignment |
|---|---|---|
| **Q4** | **Decision Capabilities** (custom attrs, items, rules) — IT Discover 10/20 kickoff, UC yes · **Dynamic content** (conditional rules) — Ideation, **Yes — OCP new SKUs** | **PA-06** OpenShift + Decisioning note · PA-05 personalization · wrap-up dynamic/conditional |
| **CY27Q1** | Projects: **Partner newsletter**, **OpenShift SKUs**, **RHEL Learn Mid** · Decision rules → Q1 prod (Lizzy) · **Priority scores / ranking formulas** — Q1 discovery (Eric/Ashleigh ↔ DG framework) · **Multilingual** — Q1 discovery | Partner UC in Focus Areas · **PA-06** · Learn Mid ↔ PA-03/04 handoff thinking · Multilingual / LocSync = **separate track** (not PA-*) |
| **CY27Q2–Q4** | Summit 2027 post-event · Conflict detection · Content optimization / experiment / conditional rules · Journey Optimize · **Path experiments** · targeting rules · conditions | Wrap-up experiments / Optimize · Event Modernization still gated · **PA-08** exit for pattern release |

**Named OpenShift UC in deck:** Pre-purchase rebuild (Taliyah) — Discover/Learn/Expand · Decisioning + dynamic content · High / Draft — matches **PA-06**.

**Still in deck as reference (not the Q4 “Feature / Status” headline):** Journey capping & arbitration explainers · AJO↔Target · unitary/business/AQ event types — keep using for **PA-02/03/04/08** design even when not the active WF status row.

---

## Other OOTB AJO B2C features to consider (nurture-framework lens)

*Beyond PA-01…08 (PA-07 unused). Deck rule still applies: not every feature needs a use case.*

### Already covered / Focus Areas–owned
| Area | Where |
|---|---|
| Decisioning + Partner newsletter | Focus Areas Q4/Q1 + architecture note |
| Dynamic content / OCP SKUs | Focus Areas Q4 Ideation + **PA-06** |
| Multilingual discovery | Focus Areas CY27Q1 · **separate track** [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md) |
| Priority / ranking formulas | Focus Areas CY27Q1 discovery |
| Path experiments / Optimize / conditions | Focus Areas CY27Q2–Q4 |
| Jump, entry model, quiet hours, weekday waits | PA-01…04 |
| Exit criteria | **PA-08** |

### Extra digs that still fit the framework (not yet Focus Areas “status” rows)

| Feature (OOTB) | Nurture-framework job | Note |
|---|---|---|
| **Reaction events** | Ladder vs catapult inside one pattern | Complements OpenShift wait→reaction sketch; better than Jump for velocity |
| **Exit criteria** (**PA-08**) | Pattern **release** | Aligns with Focus Areas Target page example (exit on trial started) |
| **Incremental Read Audience** + post-batch trigger | Scheduled marketable nurture | Hardens PA-04 for OpenShift / Learn Mid |
| **Channel rule sets** (freq + quiet hours; promo vs transactional) | Fatigue + coexistence with Marketo | PA-02; deck still notes Marketo caps aren’t shared |
| **Journey capping / concurrency** (deck § still present) | One experience per product line | Pairs with Q1 priority-score discovery |
| **Content fragments** | Shared modules across stages | Speeds GenStudio→AJO beside Decisioning |
| **AJO ↔ Target seam** (in deck) | Omnichannel Experience Pattern | Keep as pattern standard, not a new feature hunt |

### Suggested priority (aligned to Focus Areas + parking lot)
1. **Q4 now:** Decisioning (IT Discover) + **dynamic content for OCP/OpenShift** (**PA-06**)  
2. **Pattern hygiene:** **Exit criteria** (**PA-08**) + entry model (**PA-04**) + capping/priority (deck + Q1 discovery)  
3. **Inside-journey intelligence:** Reaction + conditional/dynamic (not Jump-first)  
4. **CY27Q1 discovery (platform):** ranking formulas  
5. **Defer:** Jump sprawl, extra channels until a named UC needs them  
6. **Separate track (not this backlog):** Multilingual / LocSync — [`architecture/locsync-ajo-multilingual.md`](architecture/locsync-ajo-multilingual.md)

---

## Seed themes for the later full platform analysis

Use this as a table of contents draft when the pilot has more runtime — expand only what’s still relevant.

1. **Conflict management & prioritization** — Channel rule sets (freq + quiet hours), Journey capping & arbitration, conflict detection
2. **Orchestration patterns** — weekday/weekend waits, custom Wait formulas, timezone standards, **Jump / sub-journey handoffs**, **Reaction events**
3. **Journey entry model** — Read Audience vs AQ vs unitary vs business event; scheduled nurture vs Jump-capable modules (**PA-04**)
4. **Content management** — personalization (**PA-05**), dynamic/conditional content, fragments, experiments *(multilingual / LocSync = separate track)*
5. **Events & catalog** — schemas, fields, rules (gates Event Modernization)
6. **Decisioning** — product/topic / SKU offer blocks inside shells (**PA-06** §§3–4)
7. **Nurture framework operating model** — Experience Patterns as journey units; OpenShift Pre-purchase as next pattern candidate
8. **Omnichannel seam** — Target / inbound aligned to same pattern story
9. **Measurement** — CJA views, experiments, what platform features change the scorecard
10. **Operating model** — when a feature becomes a reusable RH pattern vs journey one-off

---

## Idea intake (copy/paste)

```text
ID: PA-__
Title:
Focus Area:
Status: idea
Question / hypothesis:
Adobe feature / doc:
Pilot relevance (apply now / later / never to transactional):
Open decisions:
Explore checklist:
```

---

**Created:** 2026-11-11  
**Last updated:** 2026-11-18 — Two-track split: this file = platform expansion only; journey build → `ops/journey-build-requirements.md` · `TRACKS.md`
