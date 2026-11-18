# Promotional email — Read Audience settings (handbook contract)

**Track:** 2 — Journey build · parent [`journey-build-requirements.md`](journey-build-requirements.md) · [`../TRACKS.md`](../TRACKS.md)  
**Status:** Working note for handbook §2.5.2 / Q1 journey-entry BP  
**Evidence:** `ajo-read-audience-rhel-learn-deep.png` (local binder sources — not in this public repo) (Orchestration: Read Audience panel)  
**Handbook:** `Digital Architecture Handbook Ch2 (not in this repo)` §2.5.2  

---

## Reference instance — RHEL Learn Deep (promo)

| Setting | Value |
|---|---|
| Label | RHEL Learn Deep |
| Audience | RH CDP - Persons - AJO B2C - RHEL Learn Deep |
| Namespace | B2B Person (`b2b_person`) |
| Merge policy | Default Timebased |
| Use supplemental identifier | Off |
| Reading rate | 5,000 / sec |
| Journey schedule | **Daily** (via Edit journey schedule) |
| Incremental read | **Yes** on recurring daily — only net-new audience joiners since last run |

Also gate sends with **`profile.b2b._redhat.marketable` == true** when required ([fields inventory](aep-email-personalization-fields.md)).

---

## Contract (short)

1. Promo / standing nurture → **Read Audience** shell (not Jump-capable).  
2. Schedule type **Daily** for ongoing programs; use one-time only for single blasts.  
3. Turn on **incremental read** for daily recurrence so you do not re-pull the full audience every day.  
4. Prefer **trigger after batch audience evaluation** when the CDP audience is batch-evaluated.  
5. Namespace must match profile identity (`b2b_person` for this RHEL Persons path).  
6. Reading rate: start at 5,000; remember sandbox share of 20k/sec across concurrent Read Audiences.  

Paste-ready handbook text lives in `handbook/source.txt` §2.5.2 — sync to Google Doc when ready (`handbook/meta.yaml` workflow).
