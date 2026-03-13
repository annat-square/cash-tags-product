# Tag Transaction Notifications

## Overview

This document tracks transaction notification updates required for Cash App Tags (Physical Cash Tags / NFC). These notifications are **distinct from** fulfillment notifications or card/token lifecycle notifications.

The goal is to identify and update notifications that reference "card" language and require a Tag-specific variant or copy update, prioritized by send volume.

**Tracking sheet:** [Cash Tags × Transaction Notifications](https://docs.google.com/spreadsheets/d/1Giiv0znr2XtD-f9Rg8KkluMDxM6gTvtmIJ6HE37tcjQ/edit?gid=747792664#gid=747792664)

---

## P0 Notifications (Launch Blockers)

These are Tag-specific transaction decline scenarios requiring net-new or updated notification copy before launch.

| Campaign | Condition | Channel | Current Copy (Card) | Proposed Copy (Tag) |
|---|---|---|---|---|
| `TRANSACTIONDECLINEDCARDDISABLED` | Tag is locked | iOS Push / Android | Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Card is locked. Open the app to enable your card and try again. | Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Tag is locked. Open the app to enable your Tag and try again. |
| `TRANSACTIONDECLINEDCARDDISABLED` | Tag is locked | SMS | Cash App: Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Card is locked. Open the app to unlock your card then try again. | Your {{amount}} payment at {{bankingDescription}} was declined because your Cash App Tag is locked. Open the app to enable your Tag and try again. |
| `TRANSACTIONDECLINEDTAGINACTIVE` | Tag is provisioned, but Tag token is not active | iOS Push / Android | N/A — net new | Your Cash App Tag was declined because it's not activated. Activate your Tag in the app and try again. |
| `TRANSACTIONDECLINEDTAGINACTIVE` | Tag is provisioned, but Tag token is not active | SMS | N/A — net new | Cash App: Your Tag was declined because it's not activated. Activate your Tag in the app and try again. |
| `TAGDECLINEINVALIDPIN` | Tag is forced through pin/debit, incorrect PIN entered | iOS Push / Android | Your Cash App Card was declined because you entered the wrong Cash PIN. Tap to view your privacy & security settings. | Your Cash App Tag was declined because you entered the wrong Cash PIN. Tap to view your privacy & security settings. |
| `TAGDECLINEINVALIDPIN` | Tag is forced through pin/debit, incorrect PIN entered | SMS | Cash App: Your Cash App Card was declined because you entered the wrong Cash PIN. | Cash App: Your Tag was declined because you entered the wrong Cash PIN. |
| `TransactionDeclinedCardTerminated` | Tag is provisioned, but token is linked to inactive card | iOS Push / Android | Your {{amount}} payment at {{bankingDescription}} was declined because the card you are using is no longer active. Make sure you are using your active card and try again. | Your {{amount}} payment at {{bankingDescription}} was declined because the Tag you are using is linked to a card that is no longer active. Activate your Tag in the app with your active card. |
| `TransactionDeclinedCardTerminated` | Tag is provisioned, but token is linked to inactive card | SMS | Cash App: Your {{amount}} payment at {{bankingDescription}} was declined because the card you are using is no longer active. Make sure you are using your active card and try again. | Cash App: Your {{amount}} payment at {{bankingDescription}} was declined because the Tag you are using is linked to a card that is no longer active. Activate your Tag in the app with your active card. |

---

## Figma

Lori to add P0 notifications into Figma under a new **"Transaction Notifications"** section. Each notification should include the **condition** field to help viewers understand when Tag-specific variants are triggered.

---

## Owners

| Area | Owner |
|---|---|
| Notification copy audit & sheet | Anna |
| Eng review (existing notifs) | @tintinr / @clopezrivas |
| Figma (transaction notifications section) | @lorichoy |
