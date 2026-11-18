# AJO permissions & user onboarding (Q1) — sketch / idea dump

**Status:** Sketch · idea dump — **not** a finished SOP.  
**Purpose:** Capture tool process, documentation, and best-practice work needed so the right people can use AJO ahead of CY27Q1.  
**Source:** `[AJO] Permissions and User Onboarding for Q1.pdf` → binder copy [`../sources/ajo-permissions-user-onboarding-q1.pdf`](../sources/ajo-permissions-user-onboarding-q1.pdf)  
**Extract:** [`../sources/ajo-permissions-user-onboarding-q1-extract.txt`](../sources/ajo-permissions-user-onboarding-q1-extract.txt)  
**Parked:** 2026-11-11

---

## Source checklist (as shared)

1. **Review AJO B2C & B2B user permissions** — confirm they support the use cases needed (Q1 projects: Partner newsletter, OpenShift SKUs, RHEL Learn Mid, plus B2B track).
2. **Identify who needs platform access ahead of Q1**
   - **AJO B2C:** Lizzy Ackerman · Taliyah McBride · Tannesia · Amanda Morelli  
   - **AJO B2B:** *(blank in source — fill in)*
3. **What materials do they need to get started?** (best practices, examples of how to set up a journey)  
   Evidence crop: [`../sources/ajo-q1-materials-to-get-started.png`](../sources/ajo-q1-materials-to-get-started.png) · enablement home: [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md) §10  
   - **a.** Proof of cohort participation *(Co-Author)*  
   - **b.** Initial Best Practices *(Co-Author)*  
     - **i.** Marketable  
     - **ii.** Bouncebacks  
     - **iii.** Journey entry guidance  
     - **iv.** Namespace guidance  
     - **v.** Audience Selection *[Amanda Machado]*  
     - **vi.** Where/How to request unitary event (including template)  
     - **vii.** How to request support  
   - **c.** Troubleshooting Documentation for MarTech

---

## Idea dump — process / docs / best practices to flesh out

Treat each row as a future doc or runbook stub. Link finished artifacts here when they exist.

| Topic | Why it matters | Draft owner / note | Status |
|---|---|---|---|
| **Permissions matrix (B2C vs B2B)** | Roles must cover Decisioning, dynamic content, journeys, Translation/Multilingual discovery, reporting | DA + IT/Adobe admin | sketch |
| **Q1 user roster + access request path** | Named B2C cohort above; complete B2B list | DA lead | sketch |
| **Cohort / proof of participation** | Gate for who gets sandbox + prod-adjacent access | TBD | sketch |
| **Best practice: Marketable** | Journey condition: `profile.b2b._redhat.marketable` == true ([fields inventory](../architecture/aep-email-personalization-fields.md)) | Co-author | path verified; BP prose TBD |
| **Best practice: Bouncebacks** | Suppress / hygiene so Q1 journeys don’t re-mail bad addresses | Co-author | sketch |
| **Best practice: Journey entry** | Promo vs transactional contract: [`../architecture/journey-build-requirements.md`](../architecture/journey-build-requirements.md); promo detail ([promo settings](../architecture/promotional-read-audience-settings.md); handbook §2.5.2) | Co-author | sketch + Learn Deep reference |
| **Best practice: Namespace** | Jump + event journeys require coherent namespace (**PA-03**) | Co-author | sketch |
| **Best practice: Audience selection** | Amanda Machado — who builds vs who requests; streaming vs batch | Amanda Machado | sketch |
| **AEP fields for email personalization** | What `{{profile…}}` paths exist — living inventory | [`../architecture/aep-email-personalization-fields.md`](../architecture/aep-email-personalization-fields.md) | **WIP — need to confirm** (Marketable verified) |
| **Tag structure (AEP/CDP/AJO B2C)** | Audience name pattern + AJO Unified Tag categories | [`aep-cdp-ajo-b2c-tag-structure-sketch.md`](aep-cdp-ajo-b2c-tag-structure-sketch.md) | sketch |
| **Unitary event request (template)** | Where to file, required XDM fields — **IT builds** schema/dataset/event; DA requests + consumes. Gates exits (**PA-08**) & Trial Start–style journeys | DA requests · **IT builds** | sketch |
| **How to request support** | MarTech / DA / Adobe escalation path | MarTech | sketch |
| **Troubleshooting for MarTech** | Common publish / entry / delivery failures | MarTech | sketch |
| **Journey setup examples** | One golden-path example per entry type (scheduled nurture, unitary transactional) | DA | sketch |
| **Tie to Focus Areas UCs** | Onboard users against Partner / OpenShift / Learn Mid work, not generic tour | Align Focus Areas deck | sketch |

---

## Suggested doc set (when this graduates from sketch)

```text
ops/
  ajo-permissions-user-onboarding-q1-sketch.md   ← this file
  permissions-matrix-b2c-b2b.md                  (TBD)
  best-practices/
    marketable.md
    bouncebacks.md
    journey-entry.md
    namespace.md
    audience-selection.md
  templates/
    unitary-event-request.md
  support-and-troubleshooting.md
```

---

## Links to related binder work

- Team guide §10 (IT / process): [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md)
- Journey build (entry / namespace examples): [`../architecture/journey-build-requirements.md`](../architecture/journey-build-requirements.md)
- Pilot unitary event reference: [`../architecture/transactional-unitary-event-settings.md`](../architecture/transactional-unitary-event-settings.md)
- Platform / feature backlog: [`platform-analysis-parking-lot.md`](platform-analysis-parking-lot.md) (esp. PA-04 entry, PA-08 exit, PA-03 namespace/Jump)
- Focus Areas: `../Digital Architecture - AJO B2C_Focus Areas.pdf`
- Decisioning enablement (Partner / OpenShift authors will need catalog permissions): [`../architecture/decisioning-items-offers-requirements.md`](../architecture/decisioning-items-offers-requirements.md)

---

**Next when ready:** fill B2B user list · confirm permission packs with Adobe/IT · turn “Initial Best Practices” bullets into short co-authored pages · add unitary-event request template.
