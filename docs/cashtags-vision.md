# Cashtags Vision

> **Authors:** Anna Thompson (annat@squareup.com)
> **Status:** Working document
> **Last updated:** 2026-03-05
> **References:**
> - [Cashtags PRD (go/$tagsprd)](https://docs.google.com/document/d/12U7BErwKCxWriZh34ZwY6puFiQguAYV7N1RfIhbjZbc/edit?tab=t.0)
> - [Cashtag Modeling Strategy](https://docs.google.com/document/d/12U7BErwKCxWriZh34ZwY6puFiQguAYV7N1RfIhbjZbc/edit?tab=t.wcc58k157xfb)

---

## Purpose of This Document

This is a working document that captures the longer-term product vision and strategic direction for Cashtags. It serves two purposes:

1. **Living reference** — a place to evolve thinking on where Tags go beyond initial launch
2. **Strategy session platform** — the basis for generating an agenda for an in-person strategy session with Thomas Templeton and Hans Wang

---

## What Are Cashtags?

Cashtags are a new physical form factor enabling contactless NFC payments on any EMV point-of-sale device. They bring payments **out of the wallet**, enabling customers — especially teens and pre-teens — to tap to pay using a wearable, collectible, or accessory instead of a credit card or phone.

- **1st Party (1P):** Designed by Cash App (wands, mini cards, hearts)
- **3rd Party (3P):** Collaborations with partners (e.g., Nike, Labubu, Roblox)

The base building block is an NFC chip module ("Gem") that can be housed in virtually any physical form factor.

### Why This Matters

- Cash App Card is the fastest-growing Visa Debit Card program in US history (26M actives, 150M+ cards issued)
- 78% of teens carry a charm daily — accessories are how Gen Z/Alpha express themselves
- Cards don't align with youth culture. Tags do.
- Nothing like this has taken off in the US yet. Cash App's teen penetration (1 in 4 US teens) and proven 3P collab track record (Hood by Air, 100 Thieves, Shantell Martin) give us a unique right to win.

---

## Initial Launch (Spring–Summer 2026)

### What We're Shipping

| SKU | Target Launch | Quantity |
|-----|--------------|----------|
| Wand | Mid-May 2026 | 10,000 |
| Mini Card | Mid-June 2026 | 50,000 |
| Heart | Mid-July 2026 | 100,000 |

Followed by an **evergreen launch** of all three SKUs in August 2026.

### How It Works Today

1. Customer orders Tag in Cash App (1P ordering)
2. Customer must have an active Cash App Card (virtual or physical)
3. Tag arrives → customer provisions via NFC tap in-app
4. Tag is activated and ready to make payments on Visa debit rails

### Initial Launch Constraints

- 1 active Tag per account at a time
- 1P ordering only (Cash App)
- 1P manufacturing and fulfillment only (Block → CEVA)
- No personalization/customization
- No geolocation/tracking
- US only (works internationally like existing Cash App Cards)

---

## The Bigger Vision: Where Tags Go Next

### The Core Tension

Today's flow requires: **Cash App download → CIP/KYC → Card issuance → Card activation → Tag provisioning → Spend.**

This is extremely high friction. For Tags to become a true acquisition lever and cultural product, we need to solve for **immediate usability** — a customer should be able to walk out of Walmart with a Tag and immediately go to 7-Eleven to buy a soda, without sitting in the parking lot navigating card onboarding.

> **If customers can't spend on Tags immediately post-purchase, no one will want to buy them, and retail partners won't want to sell them.**

### Vision Pillars

#### 1. Instant Spend: Gift Card Model

The highest-leverage unlock for Tags is enabling **immediate transactability** without requiring Cash App, CIP, or card issuance upfront. Two approaches under exploration:

**Option A: Two-PAN Model (Gift Card PAN + Issued Card PAN)**
- Tag comes pre-loaded with a balance at retail POS (e.g., $50)
- Functions as a restricted prepaid card immediately — no CIP required
- Customer can check balance by downloading Cash App (or a logged-out web experience)
- When balance approaches $0, prompt for Cash App account creation → CIP → card issuance
- Re-provision Tag with issued card token
- *Trade-off:* Two PANs, split account ecosystem, moderate compliance complexity

**Option B: One-PAN Model (Gift Card PAN upgraded to Issued Card PAN)**
- Same immediate usability as Option A
- But the PAN stays the same after CIP cutover — seamless upgrade, unified account
- No re-provisioning required
- *Trade-off:* Higher development complexity, more complex compliance flows

**Why This Matters:**
- The US gift card market is **$200–$220 billion**. Tags could disrupt this by becoming the new shape of gifting.
- Immediate spend dramatically improves the retail partner value proposition
- Deferred CIP means higher conversion — customers onboard when balance runs low, not upfront
- Gifting becomes frictionless: buy a Tag, load a balance, hand it to someone

#### 2. Retail & E-Commerce Distribution

Initial launch is 1P only (ordered through Cash App). The vision is to expand across all distribution channels:

| | Block Manufacturing | 3P Manufacturing |
|---|---|---|
| **Cash App** | ✅ Launch | Phase 2 |
| **3P E-comm** (nike.com, roblox.com) | Phase 2 | Phase 3 |
| **3P Retail** (Walmart, Target) | Phase 2 | Phase 3 |

Retail distribution is critical for the gift card model — Tags need to be where customers already are.

#### 3. 3P Collaborations & Cultural Virality

Tags are designed to be **viral-ready** — unique, collectible designs that drive word-of-mouth and social media. The 3P collaboration model is central:

- Partner-led design (Nike, Labubu, Roblox, etc.)
- The Gem is the universal building block — partners design the housing
- Drop culture model: limited editions, scarcity, "sold out" and "coming soon" states that build hype
- Hardware Development Kit for partners to integrate the Gem into any form factor

#### 4. Transferability & Resale

For v2+, Tags can be wiped and re-provisioned to a new owner, enabling a **secondary market**:

- A customer can pass their Tag to a friend or sell it
- The new owner re-provisions with their own account
- Chip lifetime and expiration must be visible for resale transparency
- This turns Tags into collectibles with real secondary market value

#### 5. Tag-First Ordering

Today, a customer must have a Cash App Card before ordering a Tag. The future vision flips this:

- **Tag-first:** A customer orders (or buys at retail) a Tag as their entry point into Cash App
- The Tag becomes the acquisition lever, not the card
- Card onboarding happens after, prompted by the Tag experience

#### 6. Peer-to-Peer Gifting

A customer should be able to gift an existing Tag to a friend and load a balance:

- *Example:* I get a new Tag and want to gift my old one to my younger cousin. I load $50 from my Cash Balance so they can spend immediately, before they complete CIP or get their own issued PAN.
- This requires the gift card model (Option A or B) to work seamlessly with P2P transfers

#### 7. Future Product Extensions

| Capability | Description |
|-----------|-------------|
| Cashtag-only Offers | Exclusive offers that only work when paying with a Tag |
| Cashtag spending insights | Spending analytics specific to Tag transactions |
| Customizable transaction limits | Per-Tag spending controls |
| P2P via Cashtag | Send/receive money via Tag tap |
| POS animation / seller visibility | Visual feedback at point of sale when a Tag is used |
| Personalization / customization | Custom Tag designs |
| Multi-Tag management | Multiple active Tags per account |
| Multi-card linking | Link different Cards to different Tags |
| Desktop / web management | Manage Tags outside the mobile app |
| Pre-orders / waitlists | For limited edition drops |
| Geolocation / tracking | Find my Tag (requires battery, v2+) |

---

## Strategic Questions for Thomas & Hans

These are the big questions to drive the in-person strategy session:

### Distribution & Acquisition
1. **How aggressively do we pursue the gift card model?** Option A (two-PAN) vs. Option B (one-PAN) — what's the right trade-off between speed-to-market and long-term architecture?
2. **What's our retail distribution timeline?** When do we need to be in Walmart/Target to hit our acquisition targets?
3. **Can Tags become the primary acquisition funnel for Cash App?** What would Tag-first ordering look like at scale?

### Product & Culture
4. **What's the 3P collaboration pipeline?** Which partners do we prioritize for Fall Releases and beyond?
5. **How do we build a secondary market for Tags?** What does transferability/resale look like, and how does it interact with compliance?
6. **What's the right scarcity model?** How do we balance limited drops (virality) vs. evergreen availability (revenue)?

### Business Model
7. **What's the long-term pricing and revenue model?** Hardware margin + interchange + breakage revenue from gift card balances + float income?
8. **How do we think about Tags in the context of Cash App Pay?** Could Tags route through Cash App Pay at Square POS instead of Visa rails?
9. **What's the investment case for moving off Fidesmo / building our own chip?** When does vertical integration make sense?

### Compliance & Risk
10. **What's the regulatory path for the gift card model?** Restricted prepaid card program — what do Sutton and Bancorp need?
11. **Pre-teen (U13) accessibility** — what's the compliance and bank approval path to open Tags to younger customers?
12. **How do we handle denylisting at scale?** Manual (MVP) vs. automated API integration — what's the timeline?

---

## Agenda Framework: Strategy Session with Thomas & Hans

> *Suggested format: Half-day working session*

### Block 1: State of Play (30 min)
- Initial launch status (Wand → Mini Card → Heart)
- Key metrics and success criteria
- What we've learned from alpha/beta

### Block 2: The Acquisition Problem (45 min)
- Current funnel friction: Cash App → CIP → Card → Tag → Spend
- Gift card model deep dive (Option A vs. B)
- Retail distribution requirements and partner readiness
- **Decision needed:** Do we commit to the gift card model for 2026 H2?

### Block 3: 3P & Cultural Strategy (45 min)
- 3P partner pipeline and Fall Releases plan
- Drop culture mechanics: scarcity, virality, secondary market
- Tag-first ordering as an acquisition lever
- **Decision needed:** 3P partner prioritization for 2026

### Block 4: Long-Term Architecture (45 min)
- Transferability and resale
- Moving off Fidesmo / Visa Token Service direct integration
- Cash App Pay integration at Square POS
- Multi-Tag, multi-card future
- **Decision needed:** Technical investment priorities for 2027

### Block 5: Business Case & Investment (30 min)
- Revenue model: hardware + interchange + gift card economics
- Investment case for vertical integration
- Headcount and resource needs
- **Decision needed:** 2027 investment ask

---

## Contributors

- Anna Thompson ([@annat](https://sq-block.slack.com/team/annat)) — Product, In-App Experience
- Hans Wang (hans@squareup.com) — Product Lead, PRD Author
- Ricky Graziosi (rgraziosi@squareup.com) — Tokenization & Technical Architecture
- Nancy Schempp (nschempp@squareup.com) — Modeling Strategy
