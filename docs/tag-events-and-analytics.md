# Cash Tags: Events & Analytics

**Author:** Anna Thompson
**Updated:** March 13, 2026
**Status:** Draft — identifying key metrics and required events for eng

---

## Purpose

This doc defines the key metrics we need to track for Cash Tags, and the events engineering needs to build to power them. The goal is to ensure we have full visibility into the Tag lifecycle — from ordering through activation, usage, and churn — so we can measure product health, inform decisions, and report to stakeholders.

---

## Key Metrics

### Acquisition & Activation

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag orders | Total Tags ordered | `tag_ordered` |
| Tag order completion rate | % of customers who start Tag order flow and complete it | `tag_order_flow_started`, `tag_ordered` |
| Tag shipment → delivery time | Days from shipment to delivery | `tag_shipped`, `tag_delivered` |
| Tag activation rate | % of delivered Tags that are activated (provisioned) | `tag_delivered`, `tag_activated` |
| Time to activate | Days from delivery to first activation | `tag_delivered`, `tag_activated` |
| Tag activation method | Breakdown by activation method (Fidesmo app vs in-app) | `tag_activated` (with `method` property) |

### Usage & Engagement

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag actives (DAU / WAU / MAU) | Unique customers who made ≥1 Tag transaction in period | `tag_transaction_completed` |
| Tag GPV | Total gross payment volume via Tag transactions | `tag_transaction_completed` (with `amount`) |
| Tag transactions per active | Avg transactions per active Tag user per period | `tag_transaction_completed` |
| Tag GPV per active | Avg GPV per active Tag user per period | `tag_transaction_completed` (with `amount`) |
| Tag vs Card GPV share | % of total card GPV that comes from Tag transactions | `tag_transaction_completed`, existing card txn events |
| Tag transaction success rate | % of Tag tap attempts that result in a successful transaction | `tag_transaction_attempted`, `tag_transaction_completed` |
| Tag decline rate | % of Tag transactions declined, by decline reason | `tag_transaction_declined` (with `reason`) |
| Tag decline reasons breakdown | Distribution of decline reasons (locked, insufficient funds, terminated, etc.) | `tag_transaction_declined` (with `reason`) |
| Merchant category distribution | Top MCCs for Tag transactions | `tag_transaction_completed` (with `mcc`) |

### Retention & Churn

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag retention (D7 / D30 / D90) | % of Tag activators who transact again in period | `tag_activated`, `tag_transaction_completed` |
| Tag churn rate | % of previously active Tag users with no Tag txn in 30d | `tag_transaction_completed` |
| Tag deactivation rate | % of activated Tags that are deprovisioned | `tag_deactivated` |
| Tag deactivation reasons | Why customers deactivate (voluntary, card terminated, card expired, etc.) | `tag_deactivated` (with `reason`) |

### Card ↔ Tag Relationship

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Tag + Card dual usage rate | % of Tag actives who also use their physical card in same period | `tag_transaction_completed`, existing card txn events |
| GPV lift from Tag | Incremental GPV from Tag customers vs matched non-Tag cohort | Requires experiment or causal inference — no new event |
| Card active → Tag conversion | % of card actives who order and activate a Tag | `tag_ordered`, `tag_activated`, existing card active definition |
| Tag-only actives | Customers who are Tag active but not card active | `tag_transaction_completed`, existing card txn events |

### Tag Health & Reliability

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Provisioning success rate | % of provisioning attempts that succeed | `tag_provisioning_attempted`, `tag_provisioning_succeeded`, `tag_provisioning_failed` |
| Provisioning failure reasons | Distribution of failure reasons | `tag_provisioning_failed` (with `reason`) |
| NFC tap success rate | % of NFC taps that result in a transaction attempt | `tag_nfc_tap`, `tag_transaction_attempted` |
| Tag replacement rate | % of Tag customers who request a replacement | `tag_replacement_requested` |

### Families / Sponsored Accounts

| Metric | Definition | Event(s) Needed |
|---|---|---|
| Teen Tag orders | Tags ordered by sponsored accounts | `tag_ordered` (with `is_sponsored`) |
| Sponsor lock rate (Tag) | % of teen Tags locked by sponsor | `tag_locked_by_sponsor` |
| Sponsor unlock rate (Tag) | % of sponsor-locked Tags that are subsequently unlocked | `tag_unlocked_by_sponsor` |

---

## Event Schema (Draft)

All events should follow existing Cash App event conventions. Properties listed are the minimum required — eng may add additional properties as needed.

### Lifecycle Events

```
tag_ordered
  - customer_token
  - tag_sku
  - order_id
  - timestamp

tag_shipped
  - customer_token
  - order_id
  - tracking_id
  - timestamp

tag_delivered
  - customer_token
  - order_id
  - timestamp

tag_activated
  - customer_token
  - tag_token
  - method (fidesmo | in_app)
  - timestamp

tag_deactivated
  - customer_token
  - tag_token
  - reason (voluntary | card_terminated | card_expired | sponsor_action | admin)
  - timestamp

tag_replacement_requested
  - customer_token
  - tag_token
  - reason (lost | damaged | other)
  - timestamp
```

### Transaction Events

```
tag_transaction_attempted
  - customer_token
  - tag_token
  - amount
  - merchant_name
  - mcc
  - timestamp

tag_transaction_completed
  - customer_token
  - tag_token
  - transaction_id
  - amount
  - merchant_name
  - mcc
  - timestamp

tag_transaction_declined
  - customer_token
  - tag_token
  - amount
  - merchant_name
  - mcc
  - decline_reason (insufficient_funds | card_disabled | tag_inactive | card_terminated | invalid_pin | merchant_blocked | unsupported_currency | other)
  - timestamp
```

### Provisioning Events

```
tag_provisioning_attempted
  - customer_token
  - tag_token
  - method (fidesmo | in_app)
  - timestamp

tag_provisioning_succeeded
  - customer_token
  - tag_token
  - method
  - timestamp

tag_provisioning_failed
  - customer_token
  - tag_token
  - method
  - reason
  - timestamp
```

### Families Events

```
tag_locked_by_sponsor
  - customer_token (teen)
  - sponsor_token
  - tag_token
  - timestamp

tag_unlocked_by_sponsor
  - customer_token (teen)
  - sponsor_token
  - tag_token
  - timestamp
```

---

## Open Questions

| # | Question | Owner |
|---|---|---|
| 1 | Can we distinguish Tag vs Card transactions at the Marqeta level, or do we need to infer from the token type? | Eng (Carlos / John Wu) |
| 2 | Do we need separate Amplitude events or can we add a `payment_device` property to existing card events? | Eng / PDS |
| 3 | What existing card events can we reuse vs what needs to be net-new? | Eng |
| 4 | Do we need real-time dashboards for launch monitoring, or is batch (Snowflake) sufficient? | PDS / Anna |
| 5 | How do we attribute GPV lift from Tags — experiment holdout or causal inference? | PDS |

---

## Owners

| Area | Owner |
|---|---|
| Metrics definition & prioritization | Anna |
| Event schema review | Eng (Carlos / John Wu) |
| Dashboard & reporting | PDS |
| Families-specific events | Eng (Families team) |
