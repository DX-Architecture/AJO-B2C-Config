# LocSync ↔ AJO Multilingual (separate track)

**Status:** Separate from AJO B2C **platform expansion** / parking lot. Do not fold into PA-* analysis unless product explicitly asks to merge tracks.

**Why separate:** LocSync is an RH translation backend (+ Express PoC). AJO Multilingual is locale storage/selection in Journey Optimizer. Compatibility work is ops/content localization — not the same as Decisioning, entry/exit, capping, or OpenShift pattern build.

**Related**
- Platform expansion backlog (no LocSync): [`../platform-analysis-parking-lot.md`](../platform-analysis-parking-lot.md)
- Personalization tokens (DNT through MT): [`aep-email-personalization-fields.md`](aep-email-personalization-fields.md)
- Focus Areas still lists **Multilingual → Q1 discovery** as a DA feature horizon; this file holds the LocSync depth

---

## LocSync 2.0 vs AJO Multilingual content


**Question:** Is RH’s internal LocSync compatible with AJO’s **Multilingual content** feature?

**Short answer:** **Yes as a translation engine you call, then load into AJO locale variants — not as AJO’s built-in Translation Provider.** New detail: LocSync exposes a real **HTTP backend**; the PDF is an **Adobe Express add-on** integration pattern, not an AJO connector.

**Sources (app / internal RH shares)**
- Backend API contract (Express partnership tech depth): [`sources/locsync-translation-backend-integration-guide.pdf`](../sources/locsync-translation-backend-integration-guide.pdf) · extract `.txt`
- **Proposed solution write-up — LocSync × Adobe Express** (Plan A add-on + Plan B SRT): [`sources/locsync-backend-adobe-express-proposed.pdf`](../sources/locsync-backend-adobe-express-proposed.pdf) · extract `.txt`  
  *This is Express/creative video localization — not an AJO Multilingual connector. Same LocSync MT API underneath.*

| | **LocSync translation backend** | **AJO Multilingual** |
|---|---|---|
| Job | Async translate **numbered text chunks** via AutoML and/or Gemini + Sheets PE rules / DNT / personas | Store **locale variants** in one journey/campaign; select at send via profile language / rules |
| Interface | FastAPI: `POST /translate_chunks_start` → poll `GET /translate_chunks_status/{job_id}` (`X-Api-Token`) | Manual Import HTML / edit per locale · or Automated send to onboarded provider |
| Clients today / proposed | LocSync **Streamlit** UI (internal); proposed **Adobe Express Add-on** (UI iframe `fetch` → Document Sandbox write-back) | AJO Email Designer / Language settings |
| AJO “Send to translate”? | **No** — not an Adobe Translation Provider | Requires onboarded provider list |

**What the Express guide actually proves**
- LocSync is **API-first**: chunks `{ "id": "text" }` in → `translations` map out (max 2000 chunks, 5000 chars each).
- Methods: `google` · `google+style` · `gemini` · `gemini+style` (PE rules from Google Sheets).
- Express pattern: Add-on UI reads text nodes → calls API → writes translated text back to canvas. **Gaps called out:** CORS not set, API is **internal OpenShift**, token distribution to add-on users.
- That is a **client integration recipe**. The same API could be called by Streamlit, a DA script, or a future AJO-adjacent tool — Express is the worked example, not the only consumer.

**Implication for AJO**
- **Does not** mean LocSync “works inside AJO Multilingual” the way Express might write to a canvas.
- **Does** mean “works today” can be Streamlit/API → paste/import into AJO **or** a thin ops script that uses the same chunk API, then a human (or future automation) loads locale HTML in AJO.
- Closing Express-style gaps (public HTTPS + CORS + tokens) would also unblock non-Express clients; it still wouldn’t register LocSync as Adobe’s multilingual provider without separate Adobe onboarding.

#### Express docs = PoC / conversation only (not a committed build)

Internal shares (`locsync-backend-adobe-express-proposed.pdf` + backend guide) are a **potential proof of concept / discussion**: could RH LocSync be supported *inside Adobe tech* (Express add-on as the worked example: Plan A in-canvas translate; Plan B SRT→Premiere). Effort numbers in the doc are illustrative. **Not** a DA roadmap item; **not** AJO Multilingual scope. Keep as reference that the LocSync API is client-ready enough for Adobe-side experiments.

#### Possible automated LocSync → AJO path (also exploratory — separate from Express)

Same idea at a higher level (“can internal LocSync plug into Adobe?”) applied to journeys: *plausible* because of `translate_chunks_*`, but **not** designed in the Express PoC. Options if we ever dig in:

| Approach | What you’d build | vs Express doc |
|---|---|---|
| **1. Ops automation (fastest)** | Script/service: pull EN strings from email → LocSync API → assemble locale HTML → author still Import HTML (or paste). Semi-automated. | Reuses same API; no Adobe add-on |
| **2. Custom AJO-adjacent tooling** | Internal app that batches journey email locales against LocSync (like Streamlit, purpose-built for email chunks / section templates). | Same pattern as Express client, different write-back target |
| **3. Adobe Translation Provider onboarding** | LocSync implements Adobe’s provider contract → appears in AJO Providers → **Translation project** + **Send to translate**. True in-product automation. | Highest bar; Express CORS work does **not** equal this |

Shared prerequisites with Express (if any browser/public client): reachable HTTPS API, auth/token model, DNT for `{{profile…}}`. Express CORS alone does not automate AJO.

#### Works today (detailed) — LocSync → AJO manual multilingual

AJO’s **manual translation** flow is explicitly built for “translate in the designer **or import existing multilingual content**” ([docs](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/content-management/content-multilingual/multilingual-manual)). LocSync is the “existing multilingual content” engine on the RH side.

```text
EN source (GenStudio / AJO Email Designer)
        │
        ▼
   LocSync 2.0  →  localized HTML/copy per language (TM + AutoML + Gemini)
        │
        ▼
AJO Language settings (Manual) + locale variants
        │
        ▼
Send: AJO picks locale from profile language attr / rules (+ fallback)
```

**A. One-time / infrequent AJO setup (DA / admin)**
1. Permission: **Manage Language settings** (manual flow).
2. Ensure locales exist: Content management → **Translation** → Locale dictionary → Add locale (language + region) as needed.
3. **Administration → Channel → General settings → Language settings → Create**
   - Type: **Manual Translation** (not “Translation project”).
   - Attach locales (min 2, max 50).
   - **Fallback** required — if none, journey/campaign will not send.
   - Sending preference:
     - **Profile language preference attributes**, or
     - **Custom conditional rules** (location / prefs / etc.).
   - For **email**, don’t rely on web-only contextual language events; use profile attributes (or rules with real profile/audience attrs). Drag locales to set priority.
   - Submit. (Active settings aren’t freely editable — duplicate to change.)

**B. Per journey / campaign content loop (ops + LocSync)**
1. Build the journey/campaign message in **source language** first (EN). Add personalization (see parking lot PA-05) / structure as you want the master.
2. Save → Actions → **Add languages** → select your Manual Language settings.
3. Assign the existing authored content to the source locale in the locale dropdown.
4. Locales menu → advanced → **Copy to all locales** (duplicates master into every locale).  
   - Guardrail: full email payload ≤ **2 MB** (Adobe recommends **&lt; 1 MB**); for large HTML, copy fewer locales at a time.
5. For each target locale:
   - Open locale → **Edit email body**.
   - Either hand-edit, **or** replace body via **Import HTML** / paste with LocSync output for that language.
6. Preview each locale; disable unused locales via More actions if needed.
7. Review / activate (approval policy may still apply). Reporting measures multilingual performance after send.

**C. Where LocSync sits in the loop (RH-specific)**
| Step | Owner | Notes |
|---|---|---|
| Export strings/HTML from AJO or GenStudio | Content / DA | Prefer chunkable copy; keep `{{profile…}}` as DNT or out of chunks |
| Translate via Streamlit **or** `POST /translate_chunks_start` + poll | Marketing Content / DA script | Same backend the Express guide describes; `gemini+style` + `enable_dnt` for brand |
| Optional human QA | Localization Leads | High-vis journeys |
| Import/paste per AJO locale variant | Journey author | Required AJO step today (no write-back SDK like Express Document Sandbox) |
| Locale selection at send | AJO | Profile language attribute or custom rules + fallback |

**D. Express pattern vs AJO pattern (same backend, different client)**

| | Adobe Express (guide) | AJO Multilingual (our use) |
|---|---|---|
| Read source | Document Sandbox text nodes | Email Designer / exported HTML / GenStudio |
| Call LocSync | Add-on UI `fetch` + poll | Streamlit UI **or** script/`fetch` with `X-Api-Token` |
| Write result | Sandbox sets `fullContent.text` on canvas | **Manual:** Edit locale → Import HTML / paste · **Future:** ops automation still lands in AJO locale store |
| Blockers in guide | CORS, public Route, token to add-on users | Same if browser client; Streamlit on internal network already works with internal `MT_API_URL` |

**E. What “compatible” does and does not mean day-to-day**
- **Does:** One journey, many locale bodies; LocSync API/UI produces strings; AJO stores and selects them.
- **Does:** Chunk IDs can map to email sections (`section1_headline`, etc.) — good fit for OpenShift modular templates (parking lot PA-06) when that build is ready — still a content ops concern, not a reason to merge tracks.
- **Does not:** Express-style in-product write-back inside AJO Email Designer.
- **Does not:** AJO **Send to translate** → LocSync without Adobe provider onboarding.
- **Must:** DNT / don’t send personalization tokens through MT; leave `{{…}}` intact (`enable_dnt` + Sheets DNT lists help).
- **Watch:** Jobs are **in-memory** and deleted after first completed/error poll — capture translations immediately; 429 if too many active jobs.

**F. Good fit use cases**
- CY27 multilingual Focus Area where RH TM/PE rules matter more than Adobe’s stock providers.
- Section-based emails (chunk per headline/body/CTA) → LocSync → reassemble → locale import.
- Parallel track: Express add-on for creative; AJO for journey send — shared LocSync backend, different write-back.

**Explore later** (only if multilingual / LocSync rises as a real priority)
- [ ] Treat Express materials as PoC reference, not delivery plan
- [ ] If pursuing AJO automation later: ops script vs Adobe provider onboarding (ask app team)
- [ ] Until then: manual LocSync → AJO locale import remains the only assumed path
- [ ] Document RH locale codes + profile language attribute when we actually enable Multilingual

---
