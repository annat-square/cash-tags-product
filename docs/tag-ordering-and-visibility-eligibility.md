# Tag Ordering Eligibility — Requirements Definition

> **Source:** [#proj-mint Slack thread](https://sq-block.slack.com/archives/C08QQ6KQ8PJ/p1772661234050569) (March 4–5, 2026)
> **Living doc:** [Google Doc version](https://docs.google.com/document/d/1LQ5AWPfWYwY44Ju_v6LbTytmhBe2xI71rReTCp_vOb0/edit)
> **Last updated:** 2026-03-05

---

## Overview

This document defines the eligibility checks for the Tag ordering flow, organized into three categories:

1. **True Eligibility Checks** — Fail the flow entirely
2. **Visibility Checks** — Hide Tags from ineligible customers
3. **Visible But Not Orderable** — Show in Grid/PDP but block ordering

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
| 8 | Active sponsorship required | If sponsored account, sponsorship must be active (no pending sponsorships) |
| 9 | 1 Tag order per SKU per account | May–July launch window limit. E.g., a customer can order 1 first edition wand AND 1 first edition mini card, but NOT 2 of the same SKU |

> ⚠️ **Open:** Still ironing out with FinOps whether there will be any limitation on Stack or Prepaid/Debit Flex beyond the per-SKU limit. *(Owner: Anna)*

---

## 2. Visibility Checks (Hide Tags from Ineligible Customers)

These checks prevent the customer from even **seeing** Tags in the Grid.

| # | Check | Description |
|---|-------|-------------|
| 1 | U13 customers | Tags should NOT be visible in Grid for U13 |
| 2 | Taply inventory availability | Taply controls this by not returning inventory to Whimsicard |

### Design Note

The `selectable_payment_devices` parameter in the CDL plasma flow was originally about orderability but was reframed for displayability. One option: Whimsicard always calls both services to fetch the full device list, then marks ineligible ones as non-selectable in the UI. *(Raised by John)*

---

## 3. Visible But Not Orderable (Show in Grid/PDP, Block Ordering)

Customer can **see** the Tag in Grid and PDP but **cannot** complete an order.

| # | Check | Description |
|---|-------|-------------|
| 1 | Customer does NOT have an active card | Grid + Wand PDP should still be visible for non-cardholders |

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

---

## Policy Approach

**Jonathan's recommendation:** Create a **new** eligibility policy with the same rules as the Cash App Card equivalent, so we have flexibility to diverge if Tag-specific requirements emerge later.

---

## Contributors

- Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))
- John Kang ([@johnkang](https://sq-block.slack.com/team/johnkang))
- Carlos Lopez Rivas ([@clopezrivas](https://sq-block.slack.com/team/clopezrivas))
- Jonathan Ho ([@jonathanho](https://sq-block.slack.com/team/jonathanho))
