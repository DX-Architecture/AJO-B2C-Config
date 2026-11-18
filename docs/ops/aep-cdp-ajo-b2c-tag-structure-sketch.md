# Tag structure — AEP / CDP / AJO B2C (sketch)

**Status:** Sketch / idea dump for governance — not a finished taxonomy.  
**Purpose:** Align how we **name audiences** (AEP/RT-CDP) vs **tag journeys & related objects** (AJO Unified Tags) so inventory stays filterable without stuffing metadata into names.  
**Related:** handbook §2.9 Names, tags, nodes · [`../architecture/promotional-read-audience-settings.md`](../architecture/promotional-read-audience-settings.md) · Q1 onboarding BPs · Focus Areas UCs  

**Adobe:** [Manage tags in journeys](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/orchestrate-journeys/manage-journey/tags) · [Unified Tags overview](https://experienceleague.adobe.com/en/docs/experience-platform/administrative-tags/overview)

---

## Principle (stack-wide)

| Layer | Put metadata here | Keep the display name about… |
|---|---|---|
| **AJO** (journeys, campaigns, fragments, …) | **Unified Tags** + tag **categories** | Customer milestone / Experience Pattern job |
| **AEP / RT-CDP audiences** | Controlled **name segments** (and tags if used on segments) | Who is in the set |
| **Do not** | Encode BU + channel + sandbox + quarter into one giant string | — |

Handbook: *Tags / tag categories = Destination (Marketo · Target · AJO-B2C · AJO-B2B · CJA), BU, channel, sandbox, quarter. Journey name = milestone (e.g. RHEL trial abandonment).*

---

## 1. AJO B2C — proposed tag categories

Admin: **Administration → Tags**. Practitioners: pick **one value per category** on journey properties (add more categories only when needed).

| Category | Purpose | Example tag values (draft) |
|---|---|---|
| **Destination** | Activation / delivery / measurement system | `Marketo` · `Target` · `AJO-B2C` · `AJO-B2B` · `CJA` |
| **BU / Product line** | Framework product ownership | `RHEL` · `OpenShift` · `Ansible` · `Partner` · `Cross-portfolio` |
| **Experience Pattern / stage** | Nurture framework | `Discover` · `Learn` · `Evaluate` · `Adopt` · `Expand` · `Pre-purchase` · `Trial-Abandonment` · `Trial-Start` |
| **Channel** | Primary outbound | `Email` · `Email+Target` · `Push` · … |
| **Motion** | How it enters | `Promo-ReadAudience` · `Transactional-Unitary` · `AudienceQualification` |
| **Sandbox / env** | Where it lives | `Prod` · `NonProd` · *(name the actual sandbox)* |
| **Quarter / wave** | Planning filter | `CY26Q4` · `CY27Q1` · … |
| **Owner / WG** *(optional)* | Support path | `DX-DA` · `App-Platform` · … |

**Journey name examples (milestone only)**  
- `RHEL Learn Deep`  
- `RHEL Product Trial Abandonment`  
- `RHEL Product Trial Start`  
- `OpenShift Pre-purchase`  

Tags carry: Destination=`AJO-B2C` + `RHEL` + `Learn` + `Email` + `Promo-ReadAudience` + `Prod` + `CY26Q4`.

---

## 2. AEP / CDP — audience naming pattern (observed)

Reference: `RH CDP - Persons - AJO B2C - RHEL Learn Deep`

| Segment (left → right) | Meaning | Notes |
|---|---|---|
| `RH CDP` | Org / platform prefix | Keep stable |
| `Persons` | Profile class / entity | vs Accounts, etc. |
| `AJO B2C` | Downstream consumer / program family | Distinguishes from Marketo-only or B2B buying-group audiences |
| `{Milestone or pattern}` | Business audience job | e.g. `RHEL Learn Deep`, `RHEL RHPT Abandonment` |

**Draft convention**

```text
RH CDP - Persons - AJO B2C - {Product} {Pattern or journey milestone}
```

Optional later: add Unified Tags on segments that mirror AJO categories (BU, stage) for cross-object search — only if admins enable/govern them; do not fork a second uncontrolled vocab.

**Namespace note:** RHEL promo Read Audience uses **B2B Person (`b2b_person`)** even when the audience string says AJO B2C — tags/names describe *program*; namespace describes *identity*. Document both (see promo Read Audience note).

---

## 3. Crosswalk (same program, three places)

| Concept | CDP audience name | AJO journey name | AJO tags (sketch) |
|---|---|---|---|
| RHEL Learn Deep promo | `RH CDP - Persons - AJO B2C - RHEL Learn Deep` | `RHEL Learn Deep` | Destination=`AJO-B2C`, BU=`RHEL`, Stage=`Learn`, Channel=`Email`, Motion=`Promo-ReadAudience` |
| RHEL trial abandonment | *(CLM / CDP name per live)* | `RHEL Product Trial Abandonment` | Destination=`AJO-B2C` · Stage=`Evaluate` / `Trial-Abandonment`, Motion per entry type |
| OpenShift pre-purchase | `RH CDP - Persons - AJO B2C - OpenShift Pre-purchase` *(draft)* | `OpenShift Pre-purchase` | Destination=`AJO-B2C`, BU=`OpenShift`, Stage=`Pre-purchase` / Discover–Expand |
| Marketo coexistence (ref) | *(Marketo program audience)* | — | Destination=`Marketo` |
| On-site PZN (ref) | *(Target activity / AEP→Target)* | — | Destination=`Target` |
| Measurement (ref) | — | — | Destination=`CJA` (views / pulse / scorecard artifacts) |

---

## 4. Objects that should share the taxonomy

| Object | Tag? | Name pattern |
|---|---|---|
| Journeys | Yes — full category set | Milestone |
| Campaigns | Yes | Milestone / offer |
| Audiences (CDP) | Name pattern primary; tags if governed | §2 |
| Decision items / fragments / templates | Prefer same BU + Destination tags | Short functional name |
| Channel configs | Destination + Channel + Env | — |

---

## 5. Governance (when this graduates from sketch)

- [ ] Admin creates categories + controlled values (no free-typing near-duplicates)  
- [ ] Q1 onboarding BP: “apply tags before publish”  
- [ ] Map Focus Areas UCs (Partner · OpenShift · Learn Mid) to required tag sets  
- [ ] Confirm whether CDP segment Unified Tags are in use; if yes, sync vocab with AJO  
- [ ] Paste short version into handbook §2.9 when stable  

---

**Next:** Confirm live tag categories already in the AJO sandbox (screenshot/export) and replace draft values with what’s actually configured.
