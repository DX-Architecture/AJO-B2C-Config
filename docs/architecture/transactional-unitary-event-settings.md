# Transactional email — Unitary event settings (handbook contract)

**Track:** 2 — Journey build · parent [`journey-build-requirements.md`](journey-build-requirements.md) · [`../TRACKS.md`](../TRACKS.md)  
**Status:** Working note — pilot unitary event reference (parity with promo Read Audience note)  
**Evidence:** `ajo-unitary-event-pilot-transactional.png` (local binder sources — not in this public repo) (Events / unitary event definition) · `ajo-unitary-event-fields-eye-icon.png` (local binder sources — not in this public repo) (eye control on `Events: Pilot_Unitary`)  
**Ticket:** MARTECHCBP-9868  

---

## Reference instance — Pilot unitary event (transactional)

| Setting | Value |
|---|---|
| Description | This is the unitary event for the pilot journey as per MARTECHCBP-9868 |
| Type | **Unitary** |
| Event id type | **Rule based** |
| Schema | **B2B Activity v.1** |
| Fields | 9 fields selected — **click the eye icon** at the top of the event (next to delete) to show all selected fields |
| Event id condition | The `offerID` of `campaignInteraction_c` is equal to `'7016e000001cj7JAAU'` |
| Namespace | **B2B Person (`b2b_person`)** |
| Merge policy | Default Timebased |
| Profile identifier | The `sourceKey` of `personKey` |
| Use supplemental identifier | **Off** |

Also gate sends with **`profile.b2b._redhat.marketable` == true** when required ([fields inventory](aep-email-personalization-fields.md)).

---

## Contract (short)

1. Transactional Trial Start–style → **Unitary** event entry (not Read Audience).  
2. Event id type on this pilot: **Rule based** (condition on payload — not system-generated eventID).  
3. Namespace **`b2b_person`** + profile identifier from **`personKey.sourceKey`** — must match identities on the profile.  
4. Rule condition scopes which campaign/offer interactions fire this journey (`campaignInteraction_c.offerID`).  
5. Supplemental identifier **off** unless multi-context entrance is an explicit design.  
6. No daily schedule / incremental read — entrance is event ingest.  
7. Keep marketing quiet hours / frequency caps **off** these email actions unless CX documents an exception.  
8. **Fields:** click the **eye** at the top of the event page (`Events: Pilot_Unitary`) to show all selected fields.  
9. **R&R:** **IT builds** any new unitary events (schema, dataset, streaming, AJO event config). DA authors request via the unitary-event template (Q1 BP pack) and configure journeys against the delivered event — they do not own catalog/dataset creation.

### Payload shape (Journeys sample)

Pilot-shaped `xdmEntity` uses schema  
`https://ns.adobe.com/redhat/schemas/4e5ed1dedf2c0fcaf3134378866a4a21cb0693242aba9693`  
with `_redhat.campaignInteraction_c` (`offerID`, `offerType`, `campaignName`, …) and `personKey.sourceKey`.  
`datasetId` in samples may be omitted / placeholder — **IT** sets the real landing dataset when they build the event.

Enablement overview: [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md).

---

**Created:** 2026-11-18  
**Last updated:** 2026-11-18
