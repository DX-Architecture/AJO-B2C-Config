# Experience Pattern migration order

**Purpose:** Which Omnichannel Nurture Framework patterns move to AJO B2C first — and what stays in Marketo.  
**Related:** [`marketo-vs-ajo-architecture.md`](marketo-vs-ajo-architecture.md) · [`nets-ladders-catapults-ajo.md`](nets-ladders-catapults-ajo.md) · [`sources/cy26-omnichannel-nurture-framework.png`](sources/cy26-omnichannel-nurture-framework.png)

---

## Migration rule

Move Experience Patterns where Marketo is weakest on **real-time fallout**, **activation velocity**, and **offer sprawl** — not the entire Discover→Expand map at once.

Keep **catch / release** owned by the Experience Pattern. Change only the execution platform under email (and related tactics).

---

## Priority order

| Priority | Pattern / motion | Why AJO first | Marketo stance |
|---|---|---|---|
| **1 — Live** | Learn / Evaluate: Product engagement · Trial abandonment · Trial start | Pilot proves Net + Catapult on RHEL; CJA baselines exist | Hold sibling streams; exclude overlapping sends |
| **2 — Next** | RHEL Discover + Learn Mid (pilot roadmap) | Extend proven Learn Deep → Trial Start arc | Sunset or suppress overlapping Marketo once pulse checks clear |
| **3 — CY27 build** | OpenShift pre-purchase / modular SKU content (PA-06) | New SKU map = Decisioning + dynamic content opportunity | Don’t clone Marketo per SKU if AJO shell is ready |
| **4 — After event catalog** | Event-related Tier 1 / Tier 2 / 3rd party nets | Framework wants nets; full support blocked on tiered event data | Keep guidance-only Marketo until event catalog is durable |
| **5 — Later** | Adopt / Expand: User start · Onboard · Product adoption · Renewal · Upsell · Cross-sell | Needs durable AEP signals + Customer Success alignment | Likely Marketo/CRM-heavy until profile parity is proven |
| **Defer** | Discover: Brand awareness · broad Topic interest | Lower real-time / arbitration ROI vs Learn–Adopt | Remain Marketo + paid + site PZN for now |

---

## Stage map (quick view)

```text
Discover          Learn              Evaluate           Adopt              Expand
────────          ─────              ────────           ─────              ──────
Defer (broad)     Priority 1–2       Priority 1         Priority 5         Priority 5
                  (RHEL live/next)   (trial / pipeline) (onboard/adopt)    (renew/upsell)
                  
Event-related (all stages): Priority 4 after Q4 event catalog
OpenShift pre-purchase shell: Priority 3 (CY27 / PA-06)
```

---

## Practical build order (per pattern)

| Step | Action | Architecture outcome |
|---|---|---|
| 1 | Lock catch / release on the Experience Pattern | Framework remains SoT |
| 2 | Port cadence to AJO with fixed GenStudio content | Prove deliverability + CJA vs Marketo |
| 3 | Wire correct entry (Read Audience / AQ / unitary) | Right Net vs Ladder vs Catapult mechanics |
| 4 | Add Exit criteria; avoid Jump for clean handoffs | Honor one-pattern-per-product-line |
| 5 | Introduce Decisioning only for the offer/content slot | Stop journey cloning |
| 6 | Apply Channel + Journey caps; set pattern priority | Safe coexistence as more patterns migrate |
| 7 | Suppress Marketo overlap; document pattern owner | No silent double-nurture |

---

## Cutover gates (before Marketo suppress)

- [ ] 30-day CJA pulse vs Marketo baselines (channel, content, CTA)
- [ ] Marketable / consent path verified on AJO (`b2b_person`)
- [ ] Exit criteria tested (release KPI actually stops the journey)
- [ ] Cross-system exclude list updated (AJO ↔ Marketo caps are independent)
- [ ] Pattern owner + rollback (re-enable Marketo stream) written down

---

## What stays in Marketo (near term)

- CRM / LeanData / sales-alert heavy programs  
- Broad Discover and topic streams still mapped there  
- Patterns awaiting AJO pulse-check clearance  
- B2B Person field parity source into AEP (mktoPref maps) until AJO ownership is proven  

See coexistence matrix: [`marketo-vs-ajo-architecture.md`](marketo-vs-ajo-architecture.md)
