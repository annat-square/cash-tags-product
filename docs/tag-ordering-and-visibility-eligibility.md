# Tag Ordering and Visibility Eligibility — Requirements Definition

> **Source:** [#proj-mint Slack thread](https://sq-block.slack.com/archives/C08QQ6KQ8PJ/p1772661234050569) (March 4–5, 2026)
> **Living doc:** [Google Doc version](https://docs.google.com/document/d/1LQ5AWPfWYwY44Ju_v6LbTytmhBe2xI71rReTCp_vOb0/edit)
> **Last updated:** 2026-03-05

---

## Overview

This document defines the eligibility checks for the Tag ordering flow, organized into three categories:

1. **True Eligibility Checks** — Fail the flow entirely - either before entering or at end of order flow
2. **Visibility Checks** — Hide Tags in UI (Grid/PDP) from ineligible customers
3. **Visible But Not Orderable** — Show in UI (Grid/PDP) but block ordering

---

## 1. True Eligibility Checks (Fail the Flow)

These checks block the customer from completing a Tag order entirely. All existing card ordering eligibility checks apply, plus Tag-specific additions.

### Inherited from Card Ordering Flows

| # | Check | Description |
|---|-------|-------------|
| 1 | Denylisted account check | Account is on the denylist |
| 2 | Sponsorship suspension / restriction check | Account has sponsorship-related restrictions |
| 3 | KYB / KYC check | Know Your Business / Know Your Customer verification |
| 4 | Source of Funds review | Source of funds has not passed review |
| 5 | C4B business account check | Cash for Business account restriction |

### Tag-Specific Checks

| # | Check | Description |
|---|-------|-------------|
| 6 | No U13 customers | Also enforced at grid level (see Visibility Checks) |
| 7 | Customer must have an active card | Required to **order**, not to **see** (see Visible But Not Orderable) |
| 8 | Active sponsorship required | If sponsored account, sponsorship must be active (no pending sponsorships). ⚡ **Differs from Cash App Card:** teens with a pending sponsorship *can* order a card, but **cannot** order a Tag. This is intentional — Tag ordering requires a fully active sponsorship. |
| 9 | 1 Tag order per SKU per account | May–July launch window limit. E.g., a customer can order 1 first edition wand AND 1 first edition mini card, but NOT 2 of the same SKU |

> ⚠️ **Open:** Still ironing out with FinOps whether there will be any limitation on Stack or Prepaid/Debit Flex beyond the per-SKU limit. *(Owner: Anna)*

---

## 2. Visibility Checks (Hide Tags from Ineligible Customers)

### Product Context: Why Visibility Matters

Tags are not just another payment device — they're a **cultural product**. The visibility and eligibility strategy is intentionally designed to support a broader product and growth vision:

#### Driving Demand & Virality
Tags follow a **drop culture** model. We *want* customers to see Tags even when they can't order them — whether the device is sold out, coming soon, or the customer isn't yet eligible. Showing Tags in these states creates **pent-up demand** and **virality**: customers talk about what they've seen, share it, and come back when inventory drops or they become eligible. The scarcity and anticipation are features, not bugs.

#### Card Acquisition Funnel
A key reason for the **"visible but not orderable"** pattern is to **incentivize non-cardholders to order a Cash App Card**. When a customer without a card sees a Tag in the Grid or on a PDP, they encounter a clear signal: *get a card, unlock this*. Tags become a carrot that drives card adoption — one of our most important growth levers.

#### TL;DR for Engineers
When implementing visibility logic, keep in mind: **showing a Tag that can't be ordered is intentional, not a bug.** The three-tier eligibility model (eligible → visible but blocked → hidden) exists to serve distinct product goals. Only hide Tags when there's a hard reason to (U13, no inventory). Otherwise, let them be seen.

### Checks

These checks prevent the customer from even **seeing** Tags in the Grid.

| # | Check | Description |
|---|-------|-------------|
| 1 | U13 customers | Tags should NOT be visible in Grid for U13 |

### Design Note

The `selectable_payment_devices` parameter in the CDL plasma flow was originally about orderability but was reframed for displayability. One option: Whimsicard always calls both services to fetch the full device list, then marks ineligible ones as non-selectable in the UI. *(Raised by John)*

---

## 3. Visible But Not Orderable (Show in Grid/PDP, Block Ordering)

Customer can **see** the Tag in Grid and PDP but **cannot** complete an order.

This is the most strategically important tier of the eligibility model. These are cases where we **deliberately** show Tags to customers who can't yet order them. The reasons:

- **Non-cardholders seeing Tags → card acquisition.** A customer without a Cash App Card who sees a Tag in the Grid hits a clear call-to-action: order a card first. Tags act as a compelling incentive to drive card adoption.
- **Sold out / coming soon states → demand and virality.** When a Tag SKU is out of stock or hasn't dropped yet, we still want it visible. This mirrors drop culture — scarcity and anticipation drive word-of-mouth, social sharing, and repeat visits. "Coming soon" and "sold out" are signals that build hype, not dead ends.
- **The general principle:** if a customer can't order but there's no compliance or safety reason to hide the Tag, **show it**. Visibility drives engagement even when ordering is blocked.

| # | Check | Description |
|---|-------|-------------|
| 1 | Customer does NOT have an active card | Grid + Wand PDP should still be visible for non-cardholders — drives card acquisition |
| 2 | Taply inventory unavailable (out of stock) | Tag SKU is sold out — still visible in Grid/PDP with a "sold out" or "coming soon" state. Taply controls inventory; when stock is depleted, the Tag should remain visible but not orderable. This drives demand and mirrors drop culture. |

**Figma reference:** [Mint SoT — Non-cardholder state](https://www.figma.com/design/v6KFI34uHF76z9ScnzvwMP/%E2%9D%A4%EF%B8%8F%E2%80%8D%F0%9F%94%A5-Mint-->-SoT?node-id=12375-320914&t=z2BEGLVKSaoQhAAa-4)

> ⚠️ **Open:** This "visible but not orderable" scenario may not be fully covered in the PDP CSI contract yet — needs client-side follow-up. *(Owner: John)*

---

## Open Questions / Action Items

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | Will there be any Stack or Prepaid/Debit Flex limitations beyond the 1-per-SKU-per-account limit? | Anna | 🔴 Open |
| 2 | Does the PDP CSI contract support the "visible but not orderable" state for non-cardholders? | John | 🔴 Open |
| 3 | Should the eligibility check inside the Taply ordering sub-flow use the same policy as the parent flow? | TBD | 🔴 Open |
| 4 | New eligibility policy vs. reuse existing card ordering policy? | Team | 🟡 Leaning new (see below) |
| 5 | Add customer experience context for each "visible but not orderable" check — what does the customer see in Grid/PDP when they can't order? (e.g., sold out state, no-card CTA, coming soon treatment) | Anna | 🔴 Open |

---

## Policy Approach

**Jonathan's recommendation:** Create a **new** eligibility policy with the same rules as the Cash App Card equivalent, so we have flexibility to diverge if Tag-specific requirements emerge later.

---

## Contributors

- Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))
- John Kang ([@johnkang](https://sq-block.slack.com/team/johnkang))
- Carlos Lopez Rivas ([@clopezrivas](https://sq-block.slack.com/team/clopezrivas))
- Jonathan Ho ([@jonathanho](https://sq-block.slack.com/team/jonathanho))
