# AEP schemas / fields for AJO email personalization

**Status:** Living inventory / sketch — **exact XDM paths must be confirmed in AEP** (Schema Registry + Personalization editor). Do not invent paths from CJA names.  
**Purpose:** One binder place for what profile (and related) fields are available — or needed — when personalizing AJO B2C emails.  
**Related:** [PA-05 personalization](../platform-analysis-parking-lot.md) · [Q1 onboarding sketch](../ops/ajo-permissions-user-onboarding-q1-sketch.md) · [Decisioning attrs](decisioning-items-offers-requirements.md) (catalog ≠ profile) · handbook event/profile contract notes  

**Sandbox / env:** _TBD — prod vs non-prod_  
**Last reviewed:** 2026-11-11 — Marketable verified in editor; **Marketo→AEP mapping v2** set as canonical reference  

**Sources**
- Editor path tree: `aep-profile-b2b-redhat-marketable-path.png` (local binder `AJO B2C Pilot/sources/` — not in this public repo)
- **Canonical mapping (v2):** `redhat-aep-data-mapping-b2b-marketo-persons-v2.pdf` (local binder `AJO B2C Pilot/sources/` — not in this public repo) (+ `-v2-extract.txt`) — schema **B2B Person**, dataflow `b2b_marketo_persons`, tenant **redhat**
- Older copy (v1, superseded for binder use): `redhat-aep-data-mapping-b2b-marketo-persons.pdf` (local binder `AJO B2C Pilot/sources/` — not in this public repo) — same core `b2b._redhat.*` set; **missing** preference-center + LeanData/SFDC rows that v2 adds

**v1 vs v2 (quick)**
| | v1 | v2 |
|---|---|---|
| Pages | 12 | 24 |
| Core send/consent/persona paths (`b2b._.marketable`, etc.) | Yes | Same set |
| Marketo Pref Center (OpenShift, RHEL, Ansible, …) | No | Yes → `_redhat.mktoPref*` |
| LeanData / SFDC lead-ish source fields | No | Yes (mostly source-side; confirm AEP paths before using) |
| Prefer for inventory | — | **Use v2** |

In mapping sheets, `b2b._.` = `b2b._redhat.`. Pref fields are documented as **`_redhat.mktoPref…`** (tenant root), not under `b2b._`.

---

## Rules of the road (AJO)

| Rule | Implication |
|---|---|
| Content personalization uses the **XDM Individual Profile** schema only | Tokens look like `{{profile…}}` from that schema (plus helpers / computed attributes if enabled) |
| **Computed attributes** can be used if published to profile | List them in the inventory below when RH enables any |
| **Dataset lookup** personalization (beta) is separate | Only after datasets are API-enabled for lookup — track in §4 |
| **Decisioning** item attributes are **not** profile fields | Catalog / `_YourOrg` offer attrs live in Decisioning docs; map via `#each` on a decision policy |
| **Unitary event** payloads are not the profile schema | Event fields can personalize *that* step’s context; durable email fields still need to be on the profile |

Adobe overview: [Get started with personalization](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/personalization/personalize)

---

## How to refresh this inventory

1. In AJO Email Designer → Personalization editor → browse **Profile attributes** (and export/screenshot if useful).  
2. Or AEP → Schemas → **XDM Individual Profile** (and RH custom field groups) → copy field paths.  
3. Drop schema export / screenshots into `(local binder evidence: AJO B2C Pilot/sources/` and update the tables below (`Status` = `verified`).  
4. Note **sandbox** and **date** in the header.

Suggested drop name: `sources/aep-individual-profile-field-export-YYYY-MM-DD.*`

---

## 1. Profile fields — verified for email personalization

*Only rows with Status = `verified` are safe to put in production copy.*

| Business name | XDM / personalization path | Type | Used by (journey / UC) | Status | Notes |
|---|---|---|---|---|---|
| **Marketable** | `profile.b2b._redhat.marketable` | boolean | Journey **condition** / send gate | **verified** | Editor tree + Marketo map (`b2b._.marketable`). Require **true**. |

**Journey condition (typical):** `profile.b2b._redhat.marketable` is **true**.

---

## 1b. From Marketo→AEP B2B Person mapping (confirm in Personalization editor)

*PDF documents these on **B2B Person** / `b2b_marketo_persons`. Your Marketable screenshot shows the same `_redhat` nest under **profile**, so treat as strong candidates for AJO — still click-confirm each in the editor before production copy. Prefix `profile.` for journey/personalization expressions.*

| Business name | Mapping path (PDF) | AJO expression (expected) | Type | Status | Notes |
|---|---|---|---|---|---|
| Marketable | `b2b._.marketable` | `profile.b2b._redhat.marketable` | boolean | **verified** | Derived marketable indicator |
| Non-marketable reason | `b2b._.nonMarketableReason` | `profile.b2b._redhat.nonMarketableReason` | string | mapping-doc | Why not marketable |
| Email suspended | `b2b._.emailSuspended` | `profile.b2b._redhat.emailSuspended` | boolean | mapping-doc | Closest to bounce/suppress hygiene in this map |
| Email suspended cause | `b2b._.emailSuspendedCause` | `profile.b2b._redhat.emailSuspendedCause` | string | mapping-doc | |
| Email suspended at | `b2b._.emailSuspendedAt` | `profile.b2b._redhat.emailSuspendedAt` | date-time | mapping-doc | |
| Marketing email consent val | `consents.marketing.email.val` | `profile.consents.marketing.email.val` | string | mapping-doc | From unsubscribed → `y`/`n` style transform in PDF |
| Marketing email consent reason | `consents.marketing.email.reason` | `profile.consents.marketing.email.reason` | string | mapping-doc | |
| Persona | `b2b._.persona` | `profile.b2b._redhat.persona` | string | mapping-doc | Customer persona categorization |
| Partner persona | `b2b._.partnerPersona` | `profile.b2b._redhat.partnerPersona` | string | mapping-doc | Partner newsletter UC |
| Language preference | `b2b._.Language_Preference__c` | `profile.b2b._redhat.Language_Preference__c` | string | mapping-doc | Multilingual / locale candidate |
| Job role | `b2b._.Job_Role__c` | `profile.b2b._redhat.Job_Role__c` | string | mapping-doc | |
| Department | `b2b._.department` | `profile.b2b._redhat.department` | string | mapping-doc | |
| Company division | `b2b._.Company_Division__c` | `profile.b2b._redhat.Company_Division__c` | string | mapping-doc | |
| Is partner | `b2b._.isPartner` | `profile.b2b._redhat.isPartner` | boolean | mapping-doc | |
| Marketo is partner | `b2b._.mktoIsPartner` | `profile.b2b._redhat.mktoIsPartner` | boolean | mapping-doc | |
| Is customer | `b2b._.IsCustomer` | `profile.b2b._redhat.IsCustomer` | boolean | mapping-doc | Confirm casing in editor |
| Is customer portal | `b2b._.IsCustomerPortal` | `profile.b2b._redhat.IsCustomerPortal` | boolean | mapping-doc | Confirm casing |
| Partner business type | `b2b._.partnerBusinessType` | `profile.b2b._redhat.partnerBusinessType` | string | mapping-doc | |
| Company name | `b2b.companyName` | `profile.b2b.companyName` | string | mapping-doc | No `_redhat` nest |
| Company website | `b2b.companyWebsite` | `profile.b2b.companyWebsite` | string | mapping-doc | |
| Account key | `b2b.accountKey` | `profile.b2b.accountKey` | object | mapping-doc | |
| Eloqua contact ID | `b2b._.eloquaContactId` | `profile.b2b._redhat.eloquaContactId` | string | mapping-doc | |
| Ext source system key | `extSourceSystemAudit.externalKey` | `profile.extSourceSystemAudit.externalKey` | object | mapping-doc | Identity-related; namespace `b2b_person` in PDF |

### Marketo preference center (v2 only) — product / topic interest signals

*Documented path shape: `_redhat.mktoPref…` (boolean). Expected in editor under profile tenant node — **confirm** whether UI shows `profile._redhat.mktoPref…` (not under `b2b`). Useful for OpenShift / RHEL / Ansible conditional content (**PA-06**).*

| Preference (display) | Mapping path (v2) | Expected expression | Status |
|---|---|---|---|
| Red Hat OpenShift | `_redhat.mktoPrefRedHatOpenShift` | `profile._redhat.mktoPrefRedHatOpenShift` | mapping-doc (v2) |
| Red Hat Enterprise Linux | `_redhat.mktoPrefRedHatEnterpriseLinux` | `profile._redhat.mktoPrefRedHatEnterpriseLinux` | mapping-doc (v2) |
| Red Hat Ansible Automation | `_redhat.mktoPrefRedHatAnsibleAutomation` | `profile._redhat.mktoPrefRedHatAnsibleAutomation` | mapping-doc (v2) |
| Linux | `_redhat.mktoPrefLinux` | `profile._redhat.mktoPrefLinux` | mapping-doc (v2) |
| Kubernetes and containers | `_redhat.mktoPrefKubernetesandcontainers` | `profile._redhat.mktoPrefKubernetesandcontainers` | mapping-doc (v2) |
| AI / Artificial Intelligence | `_redhat.mktoPrefArtificialIntelligence` (+ related `mktoPrefAI`) | `profile._redhat.…` | mapping-doc (v2) — confirm which attr the editor exposes |
| Virtualization | `_redhat.mktoPrefVirtualization` | `profile._redhat.mktoPrefVirtualization` | mapping-doc (v2) |
| Security | `_redhat.mktoPrefSecurity` | `profile._redhat.mktoPrefSecurity` | mapping-doc (v2) |
| DevOps / Cloud-native / Automation / … | other `_redhat.mktoPref*` in v2 | same pattern | mapping-doc (v2) — full list in PDF pp.2–4 |

**Caveats**
- PDFs are **Marketo persons → B2B Person**, not a full Individual Profile dump. AJO only exposes what is on the journey’s **profile** union — Marketable proves `b2b._redhat` is there; confirm prefs (`_redhat.mktoPref*`) the same way.
- Still **no** first/last name or simple work-email body field in this map — look for other schemas/mixins.
- Installed product/version footprint and event attendance are still **other dataflows**.
- LeanData / SFDC columns in v2 are mostly **source** fields; don’t invent AEP personalization paths until Columns (path) shows them.

---

## 2. Known needs still open

| Business need | Why | Likely use | Status |
|---|---|---|---|
| Work / preferred email (for tokens / Jump) | Address in copy or Trial Start params | Personalization | path TBD (not in Marketo persons map) |
| Person name (first / last) | Greeting lines | Personalization | path TBD |
| Product + version / installed footprint | OpenShift pre-purchase | Dynamic content (**PA-06**) | path TBD — other dataflow |
| OpenShift / product **intent** | Pre-purchase | Audience + conditional | **partial** — try `mktoPrefRedHatOpenShift` (v2); confirm in editor |
| Identity (email / CRM / ECID) on identityMap | Journey namespace entry | Entry | partial — `extSourceSystemAudit.externalKey` / confirm identityMap |
| Last event attended / event product interest | Event Modernization | Profile durable fields | path TBD |

---

## 3. Identity / namespace (entry, not token copy)

| Concern | Note | Status |
|---|---|---|
| Journey namespace | Must match profile identity used at entry (and Jump targets) | document per journey |
| People-based namespace only | Read Audience / Jump constraints | see PA-03 / PA-04 |

---

## 4. Computed attributes & dataset lookup (optional)

| Name | Type | Path / dataset | Status |
|---|---|---|---|
| _(none listed)_ | | | |

---

## 5. Do not confuse

| Object | Use in email |
|---|---|
| **Profile field** | `{{profile…}}` personalization / conditions |
| **Decisioning item attribute** | Inside decision policy `#each` — see Decisioning note |
| **Event payload field** | Available when journey is event-triggered; persist to profile if later emails need it |
| **CJA component / eVar** | Reporting — **not** an AJO personalization path until mapped into AEP profile/event |

---

## 6. Tie-ins

| Workstream | How this inventory helps |
|---|---|
| Q1 onboarding BPs | “What can I personalize?” cheat sheet for Lizzy / Taliyah / Tannesia / Amanda |
| OpenShift Pre-purchase (**PA-06**) | Product + version, persona, intent, footprint |
| Exit criteria (**PA-08**) | Confirm trial-started (etc.) is event vs profile |
| LocSync / multilingual *(separate track)* | Leave `{{profile…}}` untranslated (DNT) — see [`locsync-ajo-multilingual.md`](locsync-ajo-multilingual.md) |

---

**Next:** Prefer **v2** PDF going forward. Click-confirm §1b + OpenShift/RHEL `mktoPref*` in Personalization editor. Still missing: name, email address token, installed footprint, event attendance.
