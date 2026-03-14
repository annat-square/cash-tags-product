# Tag Transaction Notifications

## Overview

This document tracks transaction notification updates required for Cash App Tags (Physical Cash Tags / NFC). These notifications are **distinct from** fulfillment notifications or card/token lifecycle notifications.

The goal is to identify and update notifications that reference "card" language and require a Tag-specific variant or copy update, prioritized by send volume.

**Tracking sheet:** [Cash Tags × Transaction Notifications](https://docs.google.com/spreadsheets/d/1Giiv0znr2XtD-f9Rg8KkluMDxM6gTvtmIJ6HE37tcjQ/edit?gid=747792664#gid=747792664)

---

## P0 Notifications (May'26 Launch Blockers)

These are Tag-specific transaction decline scenarios requiring net-new or updated notification copy before launch.

### Net-New Tag Transaction Notifications

These are new notification campaigns that do not exist today. They fire on Tag-specific decline scenarios that have no card equivalent, or require a distinct Tag campaign because the card copy does not apply.

| Campaign | Condition | Channel | Current Copy (Card) | Updated Copy (Tag) |
|---|---|---|---|---|
| `TRANSACTIONDECLINEDCARDDISABLED` (Tag variant) | Tag is locked | iOS Push / Android | Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Card is locked. Open the app to enable your card and try again. | Your {{amount}} payment at {{bankingDescription}} was declined because your Tag is locked. Unlock your Tag and try again. |
| `TRANSACTIONDECLINEDCARDDISABLED` (Tag variant) | Tag is locked | SMS | Cash App: Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Card is locked. Open the app to unlock your card then try again. | Your {{amount}} payment at {{bankingDescription}} was declined because your Tag is locked. Unlock your Tag and try again. |
| `TRANSACTIONDECLINEDTAGINACTIVE` | Tag is provisioned, but Tag token is not active | iOS Push / Android | N/A — net new | Your transaction was declined because your Tag isn't activated yet. Finish activating your Tag in the app. |
| `TRANSACTIONDECLINEDTAGINACTIVE` | Tag is provisioned, but Tag token is not active | SMS | N/A — net new | Your transaction was declined because your Tag isn't activated yet. Finish activating your Tag in the app. |
| `TransactionTagDeclinedCardTerminated` | Tag is provisioned, but token is linked to inactive card | iOS Push / Android | Your {{amount}} payment at {{bankingDescription}} was declined because the card you are using is no longer active. Make sure you are using your active card and try again. | Your transaction was declined because your Tag is linked to an inactive card. Reactivate your Tag with your current Cash App Card. |
| `TransactionTagDeclinedCardTerminated` | Tag is provisioned, but token is linked to inactive card | SMS | Cash App: Your {{amount}} payment at {{bankingDescription}} was declined because the card you are using is no longer active. Make sure you are using your active card and try again. | Your transaction was declined because your Tag is linked to an inactive card. Reactivate your Tag with your current Cash App Card. |

### Modified Existing Card Transaction Notifications

These are existing card notification campaigns where the copy needs to be made generic so it works for both Card and Tag transactions. No new campaign ID is needed — the existing campaign copy is updated in place.

| Campaign | Condition | Channel | Current Copy (Card) | Updated Copy (Generic) |
|---|---|---|---|---|
| `PHYSICALCARDDECLINEINVALIDPIN` | Tag or card forced through PIN debit at POS — no instrument specificity needed | iOS Push / Android | Your Cash App Card was declined because you entered the wrong Cash PIN. Tap to view your privacy & security settings. | Your transaction was declined because your PIN was incorrect. View your security settings in the app. |
| `PHYSICALCARDDECLINEINVALIDPIN` | Tag or card forced through PIN debit at POS — no instrument specificity needed | SMS | Cash App: Your Cash App Card was declined because you entered the wrong Cash PIN. | You entered the wrong PIN. View your security settings in the app. |

---

## Owners

| Area | Owner |
|---|---|
| Notification copy audit & sheet | Anna |
| Eng review (existing notifs) | @tintinr / @clopezrivas |
| Figma (transaction notifications section) | @lorichoy |
