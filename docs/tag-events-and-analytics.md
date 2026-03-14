# Cash Tags: Events & Analytics

**Author:** Anna Thompson
**Updated:** March 13, 2026
**Status:** Draft — identifying key metrics and required events for eng

---

## Purpose

This doc defines the key metrics we need to track for Cash Tags, and the events engineering needs to build to power them. Organized by launch priority so eng can sequence instrumentation work accordingly.

---

## P0 — Launch Blockers (May'26)

### Tag Fulfillment

Need end-to-end visibility into the Tag order pipeline: submission → approval → shipment → delivery.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag orders submitted | Total Tag orders placed | `tag_order_submitted` |
| Tag order approval rate | % of submitted orders that are approved | `tag_order_submitted`, `tag_order_approved` |
| Tag orders shipped | Total approved orders that ship | `tag_order_shipped` |
| Tag shipment → delivery time | Days from shipment to delivery | `tag_order_shipped`, `tag_order_delivered` |
| Tag order funnel drop-off | Where in the order flow customers abandon | `tag_order_flow_started`, `tag_order_submitted`, `tag_order_approved`, `tag_order_shipped`, `tag_order_delivered` |

### Tag Provisioning

Activation and provisioning success — critical for understanding whether customers can actually use their Tags.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag activation rate | % of delivered Tags that are successfully provisioned | `tag_order_delivered`, `tag_provisioning_succeeded` |
| Provisioning success rate | % of provisioning attempts that succeed | `tag_provisioning_attempted`, `tag_provisioning_succeeded` |
| Provisioning failure rate + reasons | % of attempts that fail, broken down by reason | `tag_provisioning_failed` (with `reason`) |
| Time to activate | Days from delivery to first successful provisioning | `tag_order_delivered`, `tag_provisioning_succeeded` |
| Provisioning method | Breakdown by method (Fidesmo vs in-app) | `tag_provisioning_succeeded` (with `method`) |

### Tag Transactions

Cash Tag-specific transactional data — auth rates, decline reasons, risk flags.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag auth rate | % of Tag transaction attempts that are authorized | `tag_transaction_attempted`, `tag_transaction_authorized` |
| Tag transaction volume | Total Tag transactions (count + amount) | `tag_transaction_completed` (with `amount`) |
| Tag decline rate | % of Tag transactions declined | `tag_transaction_declined` |
| Tag decline reasons | Distribution of decline reasons (locked, insufficient funds, terminated, inactive, etc.) | `tag_transaction_declined` (with `reason`) |
| Tag risk flag rate | % of Tag transactions flagged for risk review | `tag_transaction_risk_flagged` |
| Tag fraud rate | % of Tag transactions confirmed fraudulent | `tag_transaction_fraud_confirmed` |

### DWT Token Identification

Ability to identify a DWT (Digital Wallet Token) specifically as a token provisioned on a Tag vs other DWT types (Apple Pay, Google Pay, etc.).

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag DWT tokens active | Count of active DWT tokens that are Tag tokens | Requires `device_type` or `token_type` property on existing DWT token data |
| Tag vs other DWT breakdown | % of active DWT tokens that are Tags vs Apple Pay vs Google Pay | Same — `device_type` on token records |

> **Open question for eng:** Can we add a `device_type` (or `token_requestor_id`) property to DWT token records to distinguish Tag tokens from other wallet tokens? Or do we need a separate Tag token table?

### Tag Token Termination

How often Tag tokens are terminated and why.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag token termination rate | % of active Tag tokens terminated per period | `tag_token_terminated` |
| Tag token termination reasons | Distribution of reasons (card terminated, card expired, card stolen, voluntary deprovisioning, admin) | `tag_token_terminated` (with `reason`) |

### Token Swap Rate (Card Lifecycle)

When a customer's underlying card changes (reissue, expiry replacement, stolen replacement), the Tag token needs to be swapped to the new card. Need to track whether this is happening successfully.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Token swap trigger rate | How often card lifecycle events trigger a Tag token swap | `tag_token_swap_triggered` (with `trigger_reason`: card_reissued, card_expired, card_stolen, card_compromised) |
| Token swap success rate | % of triggered swaps that complete successfully | `tag_token_swap_triggered`, `tag_token_swap_succeeded` |
| Token swap failure rate + reasons | % that fail and why | `tag_token_swap_failed` (with `reason`) |
| Time to swap | Latency from card lifecycle event to successful token swap | `tag_token_swap_triggered`, `tag_token_swap_succeeded` |
| Tag downtime during swap | Duration Tag is non-functional between old token termination and new token activation | `tag_token_swap_triggered`, `tag_token_swap_succeeded` |

---

## P1 — Post-Launch

### Looks Order Data

Which Tag looks (designs/SKUs) customers are ordering.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag orders by look/SKU | Breakdown of Tag orders by design | `tag_order_submitted` (with `tag_sku`) |
| Look popularity ranking | Rank of looks by order volume | `tag_order_submitted` (with `tag_sku`) |
| Look conversion rate | % of customers who view a look and order it | `tag_look_viewed`, `tag_order_submitted` (with `tag_sku`) |

### Edition Card Fulfillment

Fulfillment pipeline for edition cards (special/limited edition physical cards) — same funnel as Tag fulfillment but for cards.

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Edition card orders submitted | Total edition card orders placed | `edition_card_order_submitted` (with `card_sku`) |
| Edition card order approval rate | % of submitted orders approved | `edition_card_order_submitted`, `edition_card_order_approved` |
| Edition card orders shipped | Total approved orders that ship | `edition_card_order_shipped` |
| Edition card shipment → delivery time | Days from shipment to delivery | `edition_card_order_shipped`, `edition_card_order_delivered` |

---

## P2 — Growth & Optimization

### Usage & Engagement

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag actives (DAU / WAU / MAU) | Unique customers with ≥1 Tag transaction in period | `tag_transaction_completed` |
| Tag GPV per active | Avg GPV per active Tag user per period | `tag_transaction_completed` (with `amount`) |
| Tag vs Card GPV share | % of total card GPV from Tag transactions | `tag_transaction_completed`, existing card txn events |
| Tag transactions per active | Avg transactions per active Tag user | `tag_transaction_completed` |
| Merchant category distribution | Top MCCs for Tag transactions | `tag_transaction_completed` (with `mcc`) |

### Retention & Churn

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag retention (D7 / D30 / D90) | % of Tag activators who transact again in period | `tag_provisioning_succeeded`, `tag_transaction_completed` |
| Tag churn rate | % of previously active Tag users with no Tag txn in 30d | `tag_transaction_completed` |
| Tag deactivation rate | % of activated Tags that are deprovisioned | `tag_token_terminated` (with `reason = voluntary`) |

### Card ↔ Tag Relationship

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag + Card dual usage rate | % of Tag actives who also use their physical card in same period | `tag_transaction_completed`, existing card txn events |
| GPV lift from Tag | Incremental GPV from Tag customers vs matched non-Tag cohort | Requires experiment or causal inference — no new event |
| Card active → Tag conversion | % of card actives who order and activate a Tag | `tag_order_submitted`, `tag_provisioning_succeeded`, existing card active definition |
| Tag-only actives | Customers who are Tag active but not card active | `tag_transaction_completed`, existing card txn events |

### Families / Sponsored Accounts

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Teen Tag orders | Tags ordered by sponsored accounts | `tag_order_submitted` (with `is_sponsored`) |
| Sponsor lock rate (Tag) | % of teen Tags locked by sponsor | `tag_locked_by_sponsor` |
| Sponsor unlock rate (Tag) | % of sponsor-locked Tags subsequently unlocked | `tag_unlocked_by_sponsor` |

---

## Open Questions

| # | Question | Owner |
|---|---|---|
| 1 | Can we distinguish Tag DWT tokens from other DWT types (Apple Pay, Google Pay) at the Marqeta/token level? What property do we key on? | Eng (Carlos / John Wu) |
| 2 | Do we need separate Amplitude events for Tags or can we add a `payment_device` property to existing card events? | Eng / PDS |
| 3 | For token swap — does the VAU/PAN lifecycle flow already emit events we can consume, or do we need new instrumentation? | Eng (Amir / card lifecycle) |
| 4 | Do we need real-time dashboards for launch monitoring, or is batch (Snowflake) sufficient for P0? | PDS / Anna |
| 5 | Edition card fulfillment — can we reuse existing card fulfillment events with a `card_type` property, or do we need separate events? | Eng |

---

## Owners

| Area | Owner |
|---|---|
| Metrics definition & prioritization | Anna |
| Event schema review | Eng (Carlos / John Wu) |
| Dashboard & reporting | PDS |
| Families-specific events | Eng (Families team) |
| Token lifecycle events | Eng (card lifecycle / Amir) |
