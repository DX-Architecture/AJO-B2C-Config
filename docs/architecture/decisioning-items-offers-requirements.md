# AJO B2C Decisioning — Requirements for Decision Items & Offers

Deep-dive reference for the Q4 focus area **"Decision capabilities — discovery of feature + build rules"** and the CY27 goal **"Use case development for decisioning"** (see `../project-brief.md` §6).

**Status:** Reference + draft schema proposal + starter offer catalog. Nothing here is live — the RHEL pilot does not use Decisioning today. Prep for Q4 discovery and two scoped use cases (Partner Personalized Newsletter, Event Modernization).

**Sources:**
- `../Digital Architecture - AJO B2C_Focus Areas.pdf` (pp. 6–12, 20) — practical order of work, decision item/offer mechanics, Partner Newsletter use case
- Adobe Experience League (Decisioning / Experience Decisioning docs), pulled in the [AJO B2C Decisioning](66deacc9-1c85-42e9-87f5-4832e3397c53) chat (Sep 14–15, 2026)

---

## 1. Where items & offers sit (object model)

```text
Offers catalog (one per sandbox, schema-extensible)
  └─ Decision items          ← the offer itself: attributes, dates, priority, eligibility, capping
       └─ Collections        ← rule-based groups of items (category = X)
            └─ Selection strategy   ← collection + eligibility + ranking
                 └─ Decision policy ← runtime ask: how many items, which strategies, fallback
                      └─ Journey / campaign / email component
```

There is **one** catalog (Offers) per sandbox. You never create a second catalog — you extend the existing schema and author items against it.

---

## 2. Decision item — field requirements

Created at **Decisioning → Catalogs → Create item**.

### 2.1 Standard attributes (`_experience` — always present)

| Field | Required | Notes |
|---|---|---|
| Name | Yes | Internal identifier |
| Description | No | |
| Start date / End date | No (recommended) | Item is only deliverable in-date |
| Priority | No | Integer; higher wins when ranking method = "offer priority" |
| Tags | No | Use for exact-match grouping (collections use `CONTAINS` = exact `IN`, not substring/wildcard) |
| Content fragment reference | No | Optional link to a Journey Optimizer or AEM content fragment |

### 2.2 Custom attributes (`_YourOrg` — you define these)

Added via **Decisioning → Catalogs → Edit schema**. This is the step marked **TBD** in the Focus Areas deck — nothing is defined yet.

| Type | Use for | Constraint |
|---|---|---|
| String | Category/segment keys, URLs, codes | |
| Integer / Boolean / Date / DateTime | Numeric/logical/date filters | |
| **Decisioning Content** | Copy personalized at send time with handlebars (e.g. `{{profile.person.name.firstName}}`) | |
| **Decisioning Asset** | Public image URL | Not allowed as the last field at max nesting |
| Object | Grouping nested fields | |

Guardrails: **max 100 custom attributes** per item, **max 4 nesting levels**.

### 2.3 Eligibility (who can see this item)

| Mode | Behavior | When to use |
|---|---|---|
| Default (everyone) | No restriction | Rare in practice |
| **Audience** | Precomputed AEP segment membership — can be stale (~15–30 min Edge→Hub) | Stable traits (tier, program membership) |
| **By rule** | Evaluated live against current profile/context at decision time | Anything that can change between audience refreshes (region, recent action, real-time context) |

### 2.4 Capping (how often it can be shown) — optional, up to 10 rules per item

| Dimension | Options |
|---|---|
| Event type | decision / impression / click / custom |
| Scope | offer-level vs placement |
| Type | in total vs per profile |
| Threshold | count |
| Reset frequency | time window |

### 2.5 Lifecycle

- Save as **Draft** → **Approve**.
- Only **Approved + in-date** items are ever returned by a decision.
- Sandbox-wide limit: **10,000 decision items**.

---

## 3. Collections (the layer above items)

**Catalogs → Collections → Create collection** — a saved **rule over catalog attributes**, not a folder (e.g. `category equals yoga`).

- Max **500 items** per collection.
- `CONTAINS` = exact-value `IN`, not a wildcard — use a dedicated tag attribute for "all summer offers," not a name substring.

---

## 4. What has to exist before an offer can go live in a channel

Minimum stack for **email** (the only channel in scope for the RHEL pilot today):

1. Catalog schema with the attributes you intend to render (headline, image, CTA, etc.)
2. ≥1 **Approved**, in-date decision item
3. A collection that includes it
4. A **selection strategy** (collection + eligibility + ranking), or a single item picked directly in the policy
5. An **email placement** (required for email; separate from the policy; reporting is per-placement)
6. **Enable decisioning** on the email component — ⚠️ this clears existing content, save a template first
7. A **decision policy** on the component: name it, Number of items, strategy sequence (order = priority; group with `+` to make strategies compete), optional **fallback** (fallback ignores dates/eligibility — if you skip it and nothing qualifies, the slot is empty)
8. Attributes mapped in the Personalization editor (`_experience` for standard, `_YourOrg` for custom), inside the `#each` loop
9. Preview with **Simulate** (note: expression fragments do not render in Simulate, only in Send proof / live) before publish

### Email-specific constraints worth flagging to IT early

| Constraint | Detail |
|---|---|
| AI ranking | **Not supported** for Email inside journeys |
| Nesting | Cannot nest a policy inside a component that already has one |
| Duplication | Duplicating a journey/campaign with Decisioning **breaks** the copy — must reconfigure |
| Templates | Decision policies are **not** saved inside content templates |
| Tracking | Trackable offer links must be **Decisioning Asset** type; plain string URLs aren't tracked |
| Propagation | ~15 min (up to 30 in Canada) after changing a policy/item/collection/formula before it's live everywhere |
| Consent | Consent-policy changes can take up to 48 hours to apply |

### Hard limits relevant to items & offers

| Area | Limit |
|---|---|
| Decision items per sandbox | 10,000 |
| Custom attributes per catalog schema | 100 |
| Items per collection | 500 |
| Capping rules per item | 10 |
| Selection strategies + manual items per policy | 10 |
| Items returned per policy | 30 |
| Decision policies per email | 10 |
| Placements | 1,000 |

---

## 4a. Email placements (Decisioning deep-dive)

**Adobe docs:** [Create email placements](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/decisioning/experience-decisioning/placements) · [Create decision policies — Assign a placement](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/decisioning/experience-decisioning/decision-policies/create-decision-policy)

In **Experience Decisioning** (the current engine), a placement is an **email-only slot label** you attach to the component that shows decision items. It is *not* the offer itself, and it is *not* the decision policy. It answers: *where in the email did this offer appear?* — for layout intent, capping scope, and reporting.

> **Do not confuse with legacy Decision Management placements.** In the older Offer Library model, placements also constrained content type (HTML / image / text) and were chosen when authoring offer representations. In current Decisioning, items live in the Offers catalog with schema attributes; the email placement is assigned **after** the policy, on the email component. Use Strategy setup → **Email** placements for new work.

### What a placement is (and is not)

| Is | Is not |
|---|---|
| A named container for *where* decision items show in an email (hero, footer, card row) | The decision item / offer content |
| Required when using Decisioning in email | Required for push / SMS / code-based (those channels use policies without this email placement object) |
| Tied to an **email channel configuration** | A folder or collection |
| The grain for **performance reporting** by location | The ranking / eligibility engine (that's the policy + strategy) |

### Placement properties (create once, reuse)

**Strategy setup → Email → Create placement** (or create inline when assigning in Email Designer):

| Property | Purpose |
|---|---|
| **Name** | Meaningful slot name — this is what operators pick and what reporting pivots on |
| **Description** | Optional context |
| **Tags** | AEP Unified Tags for search/classification |
| **Channel** | Email only (today) |
| **Channel configuration** | Must associate the same (or correct) email channel config used by the journey/campaign send |

Limit: up to **1,000** placements per sandbox.

### How placement fits the email Decisioning flow

```text
Decision items + collection + selection strategy
        → Decision policy (how many items, strategies, fallback)
                → Email component (Enable decisioning)
                        → Assign placement  ← required for email
                                → Map item attributes into the layout
                                        → Reporting / capping by placement
```

After the policy is created: component properties → **Decisioning** → **Assign placement**.

### Placement patterns that matter in practice

| Pattern | What to do | Why |
|---|---|---|
| **One offer, one slot** | 1 policy, Number of items = 1, 1 placement (e.g. `Hero-Offer`) | Default discovery build |
| **Several offers in a row** | Policy inside a **Repeat Grid**; Number of items ≤ tile count; usually **one** placement for that grid | Multi-card layout |
| **Same offer in two places** (header + footer) | Use **Reuse decision output** on the second slot | Without reuse, the engine **dedupes**: each placement gets a *different* offer (or blank). Reuse shares the first policy's result |
| **Different offers in two places** | Two policies, two placements (e.g. `Hero-Offer` + `Footer-Secondary`) | Intentionally different inventory / priorities |
| **Code your own HTML** | Policy insert still requires placement selection; **1 item only** (no Repeat Grid) — Limited Availability | |

Max **10 decision policies** per email.

### Naming convention (recommended for this workspace)

Use slot + use-case + product so reporting and capping stay readable:

| Placement name | Intended use |
|---|---|
| `Email-Hero-Partner-Newsletter` | Partner Pulse / Distro / TechDev primary slot |
| `Email-Body-Partner-Secondary` | Second content block in same send (if dual-slot) |
| `Email-Hero-Event-Followup` | Event Modernization post-event nurture hero |
| `Email-Cards-Event-Related` | Repeat Grid of related follow-ups (if multi-item) |
| `Email-Hero-RHEL-Abandon` | Optional RHEL Abandonment late-stage CTA |

Create placements **before** sandbox email builds so Assign placement is a pick-list, not a blocker mid-design.

### Capping scope: offer-level vs placement

On a decision item, capping scope can be:

| Scope | Behavior |
|---|---|
| **Offer level** (default) | Cap counts once per decision event across placements — same offer in hero + footer counts as one toward the cap |
| **Each placement** | Separate counter per placement — hero can hit its cap while footer still shows the offer |
| **Specific placement** | Cap only when shown in selected placements; other placements uncapped. Supported for **Email and Web**; not for code-based / push / mobile / DM |

For Newsletter discovery: start **offer-level** so multi-program partners aren't over-messaged if the same item somehow appears twice. Use **each / specific placement** only when you intentionally want different frequency by slot.

### Reporting implication

Placement exists so you can compare decision-item performance **by location** (hero vs footer vs card row), not only by item. Journey Optimizer–authored emails track decision impressions/clicks; CJA custom views should include decision proposition fields when you build Decisioning dashboards. Empty slots (no eligible item, no fallback) still mean nothing to report for that placement on that send.

### Placement checklist (email)

1. Email **channel configuration** exists and is the one journeys will use  
2. Create named placements under Strategy setup → Email  
3. Build catalog / items / collection / strategy / policy as usual  
4. Enable decisioning on the email (save template first — content clears)  
5. Add policy → **Assign placement** on that component  
6. If same offer must appear twice → **Reuse decision output**, don't add a second competing policy  
7. If capping by slot → set item capping scope to each / specific placement  
8. Simulate with profiles; confirm reporting will break out by placement name  

---

## 5. Setup steps + starter offer catalog

Practical build order and discovery-ready example items for the two scoped use cases (plus an optional RHEL pilot slice). Do these in order — collections/rules before schema design is the usual failure mode.

### 5.1 Setup steps (practical order)

| Step | Where | What you do |
|---|---|---|
| **1. Permissions** | Admin Console | Confirm **Decision managers** (or equivalent) — Decisioning menu won't appear without it |
| **2. Design catalog schema** | Decisioning → Catalogs → Edit schema | Add custom attributes under `_YourOrg` before creating items (category keys, headline, CTA, etc.) |
| **3. Create & approve items** | Catalogs → Create item | Fill standard + custom fields, set eligibility/capping, **Approve** (drafts never deliver) |
| **4. Build collections** | Catalogs → Collections | Attribute rules that group items (e.g. `newsletterProgram equals partner-pulse`) — max 500/collection |
| **5. Selection strategy** | Strategy setup | Bundle: collection + eligibility + ranking (start with **offer priority**; formula/AI later) |
| **6. Email placement** | Strategy setup → Email | Required for email; reporting is per-placement |
| **7. Decision policy in email** | Email Designer → Enable decisioning | Number of items, strategy sequence, optional **fallback** |
| **8. Map attributes** | Personalization editor | Bind `_experience` + `_YourOrg` fields into the template (inside `#each`) |
| **9. Simulate → publish** | Preview | Test with a profile that should qualify; allow ~15 min after changes before assuming live |

Cross-check: steps 2–9 satisfy the minimum stack in §4.

### 5.2 Starter offer catalog — Partner Personalized Newsletter

**Collections (examples):** `Partner Pulse EN`, `Distributors EN`, `TechDev EN` (filter on `newsletterProgram` + `language`)

| Item name | Priority | Eligibility | Custom attrs (sketch) | What it is |
|---|---:|---|---|---|
| `PP-EN-Module-Update-Q1` | 80 | Audience: Partner Pulse members | `newsletterProgram=partner-pulse`, `partnerModule=core`, `language=en`, headline/body, `ctaUrl` | Module news block for Partner Pulse |
| `PP-EN-Channel-Spotlight` | 60 | Rule: `channel equals reseller` | same + `channel=reseller` | Channel-specific spotlight when Pulse alone isn't enough |
| `DIST-EN-Margin-Program` | 90 | Audience: Distributors | `newsletterProgram=distributors`, `language=en` | Highest-priority Distro offer |
| `DIST-EN-Training-Path` | 50 | Audience: Distributors | same | Secondary Distro content |
| `TD-EN-API-Release` | 85 | Audience: TechDev | `newsletterProgram=techdev`, `language=en` | TechDev product/API drop |
| `TD-EN-Sandbox-Invite` | 70 | Rule: TechDev + recent login | same | Hands-on invite for engaged TechDev partners |
| `FALLBACK-Generic-Partner` | — | *(fallback only)* | Generic partner thank-you / homepage CTA | Used when nothing else qualifies |

**Why these:** covers multi-program overlap (Pulse vs Distro vs TechDev), priority ranking, and a real fallback — the three things the Newsletter use case is meant to prove. Schema attrs: see §6.3.

### 5.3 Starter offer catalog — Event Modernization

**Collections (examples):** `Tier1-Followups`, `KubeCon-2026`, `MWC-2026` (filter on `eventTier` / `relatedEventId`)

| Item name | Priority | Eligibility | Custom attrs (sketch) | What it is |
|---|---:|---|---|---|
| `SUMMIT-2026-Recap-Demo` | 100 | Rule: `lastEventAttended equals summit-2026` | `relatedEventId=summit-2026`, `eventTier=1`, `offerType=recap+demo`, `product=rhel` | Summit → book a demo |
| `KUBECON-2026-OCP-Trial` | 95 | Rule: `lastEventAttended equals kubecon-2026` | `relatedEventId=kubecon-2026`, `eventTier=1`, `offerType=webinar-trial`, `product=openshift` | KubeCon → OpenShift trial |
| `MWC-2026-Ansible-Path` | 90 | Rule: `lastEventAttended equals mwc-2026` | `relatedEventId=mwc-2026`, `eventTier=1`, `offerType=partner-discount`, `product=ansible` | MWC → Ansible path |
| `WEBINAR-RHEL-DeepDive` | 70 | Rule: `eventTier equals 2` AND product interest RHEL | `eventTier=2`, `offerType=webinar-trial`, `product=rhel` | Tier-2 webinar follow-up |
| `FALLBACK-Thanks-Attending` | — | *(fallback only)* | Generic “thanks for attending” + events hub CTA | Empty-slot guard when event ID isn’t mapped yet |

**Why these:** proves one journey + many event-specific offers, tier-aware eligibility, and that high fallback rate = event-catalog gap (CY27 success signal). Schema attrs: see §6.4. Gated by Q4 event catalog data.

### 5.4 Optional — RHEL pilot late-stage (Abandonment)

Only if the Abandonment “late-stage offer” becomes catalog-driven instead of fixed GenStudio copy:

| Item name | Priority | Eligibility | What it is |
|---|---:|---|---|
| `RHEL-Trial-Complete-Registration` | 100 | Audience: RHPT Abandonment; exclude already started trial | Late-stage CTA: return and finish trial |
| `RHEL-Learn-More-SysAdmin` | 60 | Rule: persona ≈ SysAdmin | Soft educate path before hard trial CTA |
| `RHEL-Learn-More-Architect` | 60 | Rule: persona ≈ Architect | Same for Architect persona |

### 5.5 Minimal first build (Q4 discovery slice)

Don’t seed dozens of items. Build this **sandbox slice**:

1. Schema with the attributes for **one** use case first (Newsletter *or* Event — don’t mix until schema-sharing is decided in §6.5)
2. **3 approved items + 1 fallback**
3. **1 collection + 1 selection strategy (priority ranking)**
4. **1 email placement + 1 decision policy** returning 1 item
5. Simulate with one qualifying profile and one non-qualifying profile

That proves the full chain (§5.1) without waiting on IT for a production schema.

---

## 6. Applied to this project

### 6.1 RHEL pilot (live, Oct 14 2026) — no Decisioning today

Current experiences (`../project-brief.md` §2) use GenStudio-authored copy, not catalog-driven offers:

- **Product Trial Abandonment** — "late-stage offer that primes return to trial" is still open copy, not a decision item (open item in `../project-brief.md` §7; optional catalog items in §5.4).
- The on-site **trial offer** referenced alongside AJO email is served by **Adobe Target**, not AJO Decisioning (two different engines — see §6 below).

So there is no existing catalog schema to inherit from. Any decisioning build starts from zero attributes.

### 6.2 Partner Personalized Newsletter — the one scoped candidate use case

From the Focus Areas template (p. 6 of the deck). This is the only use case in the workspace that has Decisioning named as its feature focus area today.

| Field | As documented |
|---|---|
| Feature focus area | Decision Management (Offer Decisioning & Decision Policies) · Content Fragments & Dynamic Content · Offer and Decisioning (Build Rules) · Web Channel (future) · Translations/Localization |
| Why | 3 partner newsletters exist today — Partner Pulse, Distributors, TechDev. A partner can qualify for more than one. Decisioning can prioritize the most relevant content per partner by **Partner Module/Channel** and **language**; possible future requirements by **region** and/or **product** |
| DA Lead | Grace Hooper / Lizzy Ackerman |
| Priority / Status | Not yet set (template placeholders) |
| How | Working board: https://miro.com/app/board/uXjVHE6K58o= |

**Catalog schema is explicitly called out as TBD** in the source deck: *"Decisioning catalog schema is still TBD for you — design attributes before scaling offers across email + web."*

### 6.3 Draft starter schema (proposal, not yet built)

Mapping the newsletter "why" onto the `_YourOrg` custom-attribute model above — for discovery/validation with Grace and Lizzy before anything is built in the sandbox:

| Attribute | Type | Purpose |
|---|---|---|
| `newsletterProgram` | String | `partner-pulse` / `distributors` / `techdev` — which newsletter this item belongs to |
| `partnerModule` | String | Partner program module, drives collection grouping |
| `channel` | String | Partner channel segment |
| `language` | String | Localization key |
| `region` | String *(future)* | Named as a likely add'l requirement |
| `product` | String *(future)* | Named as a likely add'l requirement |
| `headline` / `body` | Decisioning Content | Personalized copy per recipient |
| `imageUrl` | Decisioning Asset | Partner-asset image |
| `ctaUrl` | String | Landing page |

**Eligibility sketch:** Audience = confirmed partner-program membership (stable). Rule = `partnerModule`/`channel`/`language` match at send time (handles a partner who qualifies for more than one newsletter — rule evaluates current context, not a stale audience snapshot).

**Capping sketch:** cap per `newsletterProgram` so a partner eligible for two newsletters in the same cycle doesn't get duplicate near-identical content — needs a decision on whether that's capping (same item) or collection/strategy design (different items, same partner) since capping is per-item, not cross-program by default.

**Open design question to bring to discovery:** is "qualifies for multiple newsletters" solved by (a) one decision policy per newsletter send with program-specific collections, or (b) one unified policy that ranks across all three programs' items and returns the single best one? (b) is closer to true offer arbitration but needs a cross-program ranking formula; (a) is simpler and may be the Q4-scope answer given "discovery of feature" is still the stated goal, not production build.

---

### 6.4 Event Modernization — attended-event nurture personalization (CY27, GH)

From `../WIP_CY27 Event Modernization_GH.pdf`. Second scoped candidate use case, distinct from the Partner Newsletter — named in the project brief's CY27 directional list as "Event Modernization_GH."

**Why:** Nurture today is generic per product/topic track. The idea is to let **which event a prospect attended** (KubeCon, Mobile World Congress, Summit, a webinar) choose the offer/content block in an otherwise shared nurture journey, instead of cloning a journey per event.

**Real dependencies (source doc is explicit these are non-hypothetical, not AI-suggested):**

| Dependency | Why it's blocking |
|---|---|
| Consistent, accurate **event-level data** at **tier 1 / tier 2 / tier 3** granularity, for actual-scale events (KubeCon, MWC) | Decisioning eligibility and the `eventTier`/`eventTrack` attributes below are only as good as this data |
| Associate time/space to prioritize this build, if it's where the business needs to pivot | Organizational — not a platform gap |

> **This is gated by the Q4 "Activities / events — event catalog" focus area** (`../project-brief.md` §6). That event catalog *is* the tier 1/2/3 data source this use case needs. Treat Event Modernization as CY27-after-Q4-catalog, not parallel work.

**Pattern (matches the object model in §1–4 above):**

- Journey = nurture cadence (interest → value → offer), same journey regardless of which event they attended
- Decisioning = which offer/content block renders
- Event attendance data = who is eligible for which item

**How it would work, mapped onto §2–4:**

1. **Capture attendance on the profile** — stream a unitary event (`eventAttended`: event ID, name, track, product). Also write durable profile fields (`lastEventAttended`, `eventProductInterest`) — the triggering payload only covers the *first* message in the same journey; day-3/day-7 sends need it persisted.
2. **Catalog items** — one decision item per follow-up (Summit recap + demo, webinar trial, partner-event discount), with custom attributes `relatedEventId` / `eventTrack`. Approve them.
3. **Eligibility** — Audience (`Attended Summit 2026`, precomputed) or decision rule (`lastEventAttended equals summit-2026`, live) — rule is the better fit here since attendance history changes and later sends need current state, not a stale snapshot.
4. **One journey, one policy** — trigger on the unitary `eventAttended` event or audience qualification; email's decision policy returns the eligible item; fallback = generic "thanks for attending" if nothing matches.
5. **Multiple events attended** — ranking (priority or formula) picks the best single follow-up; capping prevents repeating the same offer across sends.

**Draft starter schema (proposal):**

| Attribute | Type | Purpose |
|---|---|---|
| `relatedEventId` | String | Which event this offer follows up on |
| `eventTrack` | String | Topic/track within the event |
| `eventTier` | String/Integer | Tier 1/2/3 — ties back to the Q4 event catalog |
| `product` | String | Product interest surfaced at the event |
| `offerType` | String | recap+demo / webinar-trial / partner-discount, etc. |
| `headline` / `body` | Decisioning Content | Personalized follow-up copy |
| `ctaUrl` | String | Landing page for the follow-up offer |

**When *not* to use Decisioning for this** (from the source doc — keep the tool proportional to the need):

| Need | Simpler approach |
|---|---|
| Only "Thanks for joining {{eventName}}" | Plain event/profile personalization in the email, no catalog needed |
| Completely different journeys per event | Separate journeys or a condition split |
| Recommend products from a huge catalog based on sessions attended | Closer to Target Recommendations / behavioral recs, not offer Decisioning |

### 6.5 Two scoped use cases, one open sequencing question

Both Partner Newsletter (§6.3) and Event Modernization (§6.4) are real-world Decisioning candidates documented in this workspace, but they hit the same unresolved catalog-schema question from different angles — multi-program eligibility (Newsletter) vs. tiered-event eligibility (Event Modernization). Worth deciding whether Q4 discovery schema work serves both with one flexible `_YourOrg` schema (e.g. a shared `programId`/`sourceEvent` style key) or whether they're separate schema extensions entirely. Event Modernization is additionally **sequenced after** the Q4 event catalog work; Partner Newsletter is not blocked on it. Starter items for sandbox discovery: §5.2–§5.5.

---

## 7. Elevating the Omnichannel Nurture Framework with Decisioning

**Sources:** `cy26-omnichannel-nurture-framework.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo), `cy25-nets-ladders-catapults.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo), `cy25-experience-pattern-criteria.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo), `cy26-ansible-experience-map.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo)

The CY25/CY26 Omnichannel Nurture Framework already defines **what** to run (Discover → Learn → Evaluate → Adopt → Expand; activation patterns; event-related patterns; email + site PZN). Decisioning elevates **how content/offers are chosen inside those experiences** for a net-new use case — without replacing the framework.

### 7.1 What stays vs. what Decisioning changes

| Framework layer (today) | Stays | What Decisioning elevates |
|---|---|---|
| Journey stages (Discover→Expand) | Stage ownership / sequencing | Same |
| Experience Patterns (building blocks) | Pattern = unit of work with Objective / Audience / KPIs / content mix | **Content mix + messaging + destination** become catalog-driven, not hard-coded per journey |
| Nets / Ladders / Catapults | Strategic intent of each experience | Decisioning is the mechanism that picks the right offer for Fallout / Progression / Velocity |
| Event-related patterns (Tier 1/2/3rd party) | Event taxonomy + nets “established but not fully supported” | Eligibility + ranking by `relatedEventId` / `eventTier` — the Event Modernization use case |
| Digital tactics (Email, Site PZN, Chat, Paid) | Channel ownership | **Same decision policy** can feed email + Target/site so the Ansible map’s email row and web PZN row tell one story |
| Digital Architecture foundation | Platform layer | Decisioning is a DA capability packaged for IT when schema/events need work |

### 7.2 Nets / Ladders / Catapults → Decisioning jobs

| Pillar | Framework meaning | Decisioning elevation |
|---|---|---|
| **Nets (Fallout)** | Catch contacts/accounts/users who left the journey | Recovery offers in the catalog (e.g. trial-abandon CTA, “resume where you left off”); eligibility = fallout audience; capping stops hammering the same net |
| **Ladders (Progression)** | Keep slow movers progressing | Stage-appropriate next content/offer from one policy (Learn → Evaluate handoff); priority/ranking = next best step, not next clone journey |
| **Catapults (Velocity)** | Accelerate on intent/behavior signals | High-intent offers win via priority or ranking when unitary events / intent signals fire; one journey, offer slot changes |

### 7.3 Experience Pattern criteria → Decisioning objects

Framework rule: every pattern has Objective, Audience (catch), Conversion goal/KPIs (release), Content type mix / destinations / messaging (+ partner motion).

| Pattern criterion | Decisioning object |
|---|---|
| Audience (catch) | Item / strategy **eligibility** (audience or rule) |
| Content mix, destinations, messaging | **Decision items** (headline, body, CTA, asset) + **collections** |
| Partner motion / messaging | Custom attrs (`partnerModule`, `channel`, `language`) — Newsletter use case |
| Conversion / release | Journey exit + CJA KPIs; Decisioning doesn’t replace exit criteria, it improves what they see before exit |
| Objective | Still owned by the Experience Pattern — Decisioning serves it |

### 7.4 Net-new use case: Event Modernization on this framework

Today the framework **names** event-related patterns (Tier 1 Summit in/during/post; Tier 2 proprietary; 3rd party; Services/Training) but notes they are *not full support — nets established for guidance*. That gap is exactly where a Decisioning use case elevates the framework:

```text
Event attended (Tier 1/2/3 data on profile)
        │
        ├─► One post-event nurture journey (Ladder / Catapult by signal)
        │         └─ Decision policy picks offer by relatedEventId + eventTier + product
        │
        └─► Same offer theme on site PZN (Target) when omnichannel is ready
```

**Without Decisioning:** new event → new journey (or guidance-only net with no scalable build).  
**With Decisioning:** new event → new **approved catalog item(s)** + collection rule; journey + pattern stay fixed. Framework stage (typically Learn/Evaluate post-event) and Experience Pattern stay; only the offer inventory grows.

Ansible Experience Map example of the same idea for a *product* pattern (not only events): Product engagement / Trial abandonment / Product evaluation can share one journey shell while Decisioning swaps the offer (complete trial vs training vs demo) — same elevation pattern, different catalog attributes.

### 7.5 What Decisioning does *not* do for the framework

- Replace Experience Patterns or stage ownership
- Replace journey capping / arbitration (conflict across journeys — separate Q4 focus area)
- Replace Target Recommendations for large product catalogs
- Fix missing event-tier data — Event Modernization still depends on the Q4 event catalog

---

## 8. Related, don't confuse

| This workspace already distinguishes | Keep separate from items/offers |
|---|---|
| **Decisioning vs Adobe Target Recommendations** | Decisioning = governed next-best-offer (eligibility, priority, capping). Recommendations = behavioral product/content recommender. They can run together. |
| **Decisioning capping (item-level)** | ≠ Journey capping (how many journeys a profile can enter) or channel frequency capping (max sends/day) |
| **Decision rule (item/strategy eligibility)** | ≠ Journey arbitration ranking formula (which journey wins a capped slot) — both can use "ranking formulas" but at different layers |

---

## 9. Next steps (Q4 discovery)

1. Validate the draft schema in §6.3 with Grace Hooper / Lizzy Ackerman against the Miro board.
2. Decide program-scoped vs unified policy (open question above) — this determines whether you need 1 or 3 selection strategies.
3. Confirm whether `region`/`product` are in Q4 scope or CY27 (deck lists them as "may include," not committed).
4. Once schema is approved, run the **minimal first build** in §5.5 (schema → 3 items + fallback → collection → strategy → policy → placement), using starter items from §5.2 or §5.3.
5. Package Why/What/How/Dependencies/DA Lead/Priority/Status into the AJO B2C Use Case and Nurture Template for IT prioritization once the schema stabilizes.
6. Land the Q4 event catalog (schemas, fields, rules) first — Event Modernization (§6.4) cannot start eligibility design without tier 1/2/3 event data.
7. Decide schema strategy across both use cases (§6.5): one shared flexible schema vs. two separate extensions.
