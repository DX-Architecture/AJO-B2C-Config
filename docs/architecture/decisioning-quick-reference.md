# AJO B2C Decisioning — Quick reference

**Audience:** Journey authors / DA teammates.  
**Role:** Where offers, copy, and placements live — and what is (and is not) built in AJO B2C.  
**Depth:** Full requirements, setup steps, starter catalog, email placements → [`decisioning-items-offers-requirements.md`](decisioning-items-offers-requirements.md)  
**Team Guide:** [`../AJO-B2C-Team-Guide.md`](../AJO-B2C-Team-Guide.md) §11–§12

**Status:** Discovery reference. RHEL pilot does **not** use Decisioning in production today.

---

## Where does the offer live?

**As a Decision item in AJO’s Offers catalog.**

| | |
|---|---|
| **Home** | AJO B2C → **Decisioning → Catalogs** (built-in **Offers** catalog — one per sandbox) |
| **Object** | A **decision item** (name, dates, priority, eligibility, capping + custom fields) |
| **Lifecycle** | Draft → **Approve** — only approved, in-date items deliver |
| **Not** | Not the email, not GenStudio, not Target, not a Marketo asset |

Collections, selection strategies, and decision policies only **choose and place** that item. Source of truth for the offer is the **catalog item**.

---

## Where does the email copy go?

| What | Where it lives |
|---|---|
| **Fixed email chrome** (header, intro, footer, legal, layout) | AJO Email Designer (optionally drafted in GenStudio first) |
| **Offer / variable copy** (headline, body, CTA, image, URL) | On the **decision item** as catalog attributes (**Decisioning Content** for personalized text; **Decisioning Asset** for images) |
| **Wiring** | Personalization editor maps `item._YourOrg.*` (org node name varies) into the email slots |

Decisioning does **not** store a finished email. It stores **offer fields**. The email is the shell that pulls those fields in at send time.

---

## Where does the email placement live?

**In AJO Decisioning → Strategy setup → Email** (email channel only today).

A placement is a **named slot** (hero, footer, card row) assigned to the email component after the decision policy. It is for location, reporting by slot, and capping scope — not the offer content itself.

Deep-dive: [`decisioning-items-offers-requirements.md`](decisioning-items-offers-requirements.md) §4a.

---

## Built and maintained in AJO B2C?

| In AJO B2C Decisioning | Adjacent (feeds or measures) |
|---|---|
| Catalog schema, decision items, collections | AEP / RTCDP profiles, audiences, consent |
| Decision rules, ranking, selection strategies | Streamed experience / unitary events (AEP) |
| Email placements, decision policies (on the email) | GenStudio (author copy → put on items / template) |
| Email channel configuration | Adobe Target (inbound PZN — separate engine) |
| | CJA (deeper Decisioning / omnichannel reporting) |

You need a **Decision managers** (or equivalent) role for the Decisioning menu.

---

## Object chain (short)

```text
Offers catalog → Decision items → Collections
       → Selection strategy → Decision policy (in email)
       → Assign placement → Map item attributes → Send
```

Practical setup order + starter offer examples: full doc **§5**.

---

## Scoped use cases (discovery)

| Use case | Job |
|---|---|
| **Partner Personalized Newsletter** | One catalog; prioritize content when a partner qualifies for Pulse / Distro / TechDev |
| **Event Modernization** | One nurture shell; offer slot follows which event they attended (gated by Q4 event catalog / tier data) |

---

**Last updated:** 2026-11-18
