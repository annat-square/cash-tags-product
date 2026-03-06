# Edition Cards

> **Status:** Draft — for review  
> **Author:** Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))  
> **Source doc:** [Customization Domain Exploration](https://docs.google.com/document/d/1j2-zgo6heu-xwx-mK8SsHIiZ2WegpSk_MJnu76Hhm4s/edit)  
> **Slack:** [#proj-mint](https://sq-block.slack.com/archives/C08QQ6KQ8PJ)

---

## What Is an Edition Card?

An Edition Card is a **card theme + skin** where the skin design is 1:1 with the card theme. To the end customer, an Edition Card looks and feels like its own unique product — even if, on the backend, it's a base card theme with a locked-in customization image.

### Glossary

| Term | Definition |
|------|-----------|
| **SKU / Theme** | The physical piece of plastic (e.g., Pink Card, Holo Card) |
| **Skin** | A design applied on top of a card theme |
| **Edition Card** | SKU + Skin where the Skin is exclusive to that SKU. Appears as a first-class product in the Grid. Not further customizable by the customer. |

### Example

> **RuPaul Edition Card** = Pink Card (theme) + RuPaul Skin.  
> The RuPaul Skin is *only* applicable to the Pink Card. The customer sees "RuPaul Card" as its own product — they don't think of it as a Pink Card with a skin on top.

---

## Why Edition Cards?

Edition Cards serve several product goals:

1. **Partnerships & cultural moments.** Edition Cards let us collaborate with artists, brands, and cultural figures (e.g., RuPaul, Sabrina Carpenter, Love Shack Fancy) to create limited, collectible card designs that drive excitement and press.
2. **Drop culture & scarcity.** Edition Cards can be released as limited drops with stated inventory, creating urgency and virality — teens in our research specifically called out blind boxes, limited drops, and influencer seeding as things that would make them want the product.
3. **Revenue & differentiation.** Edition Cards could potentially carry different pricing in the future (open question), and they give Cash App Card a unique positioning no competitor has.
4. **Grid presence.** Edition Cards appear as first-class citizens in the Grid alongside plain cards and Tags, each with their own Product Detail Page (PDP).

---

## Product Requirements

### Core Properties

| # | Requirement | Customer Experience / Goal |
|---|------------|---------------------------|
| 1 | Edition Card = card theme + skin | The customer sees a single, cohesive product. They don't pick a card and then pick a skin — the Edition Card *is* the product. |
| 2 | Stated inventory (e.g., 10,000 RuPaul cards) | Primarily for marketing. Creates scarcity narrative. We will **not** live-track inventory in eng systems or auto-cutoff after N orders. The true inventory limitation is the underlying card theme stock (e.g., pink cards). Marketing can optionally turn off an Edition Card by directive. |
| 3 | Edition Card skins are exclusive | The RuPaul skin should **not** appear in the standard skin selection interface. Customers scrolling through skins on a basic card will not randomly see partnership illustrations. This keeps Edition Cards feeling special and distinct. |
| 4 | First-class citizen in Grid | Edition Cards appear in the Grid alongside plain cards and Tags. In the Grid, Edition Cards are displayed with their skin visible, while other cards remain plain — clarifying what the user is ordering "as is." |
| 5 | Own Product Detail Page (PDP) | The RuPaul Card has a different PDP than the plain Pink Card. Each Edition Card gets its own marketing content, hero imagery, and description. PDP priority: Tags P0, Edition Cards P1, Non-Edition Card Theming P2. |
| 6 | Not customizable | Edition Cards are ordered as-is. The customer cannot further customize an Edition Card with drawings, stamps, or alternative skins. This simplifies the initial launch scope and preserves the integrity of the partnership design. |
| 7 | Multiple Edition Cards live at once | The system supports multiple active Edition Cards simultaneously. |
| 8 | Reportable | We can report on Edition Card orders specifically (e.g., how many RuPaul cards ordered) as well as by underlying theme (e.g., all pink cards, including RuPaul). RuPaul cards count as pink cards, but other pink cards do not count as RuPaul cards. |
| 9 | Removable / sell-out logic | Edition Cards can be removed from the customer experience (sold out, retired, etc.). When removed, they should not appear in re-order scenarios. This applies at all levels: SKUs, Skins, and Edition Cards should all have filtering logic for re-orders. |

### Availability States

Edition Cards follow the same availability state model as Tags and Cards:

| State | Description | Customer Experience |
|-------|-------------|-------------------|
| **TEASER** | Well before release date | Customer sees a teaser — builds intrigue, no details yet |
| **COMING_SOON** | Approaching release date | Customer sees the Edition Card in the Grid with a "Coming Soon" treatment. Can't order, but knows it's dropping soon. Builds hype. |
| **NEW** | Just after release | Customer sees a "New" pill. Can order. |
| **AVAILABLE** | General availability | Standard orderable state |
| **OUT_OF_STOCK** | Sold out or retired | Customer sees the Edition Card in the Grid with a "Sold Out" state. Still visible — mirrors e-comm experiences and drop culture. Drives demand for future drops. |

> **Note:** Edition Card availability depends on the underlying card theme's availability from Postcard. If the base Pink Card is out of stock, the RuPaul Edition Card is also out of stock.

---

## Key Product Decisions (from #proj-mint)

These decisions have been aligned on across product, design, and eng:

1. **Edition Cards are treated as distinct SKUs** with their own inventory, separating them from generic skins which typically do not have inventory.
2. **One PDP per SKU.** The final SKU definition enforces one Product Detail Page per Edition Card.
3. **All Edition Cards are new SKUs, not merely skins**, and are not eligible for further customization (for initial launch scope).
4. **In the Grid, Edition Cards show their skin; other cards remain plain.** This clarifies what the user is ordering "as is" vs. what they can customize.
5. **The skin experience is separate from Card Studio.** Swiping through skins is distinct from the deeper customization tool to accommodate different user types.
6. **Edition Card naming over "Partnership Card."** Since Edition Cards could comprise either partner-branded skins OR Cash App brand-generated cards, the abstraction from the strict "partner" word is intentional.

---

## Engineering Architecture (Summary)

> Full eng design lives in the [source doc](https://docs.google.com/document/d/1j2-zgo6heu-xwx-mK8SsHIiZ2WegpSk_MJnu76Hhm4s/edit).

### Source of Truth

**Postcard** is the recommended SOT for Edition Cards (not Whimsicard). Rationale: Edition Cards are closer to card themes than to skins. Making Whimsicard the SOT would mean the Skins-owning service dictates orderability of the underlying Card Theme, breaking Postcard's role as the Card Theme SOT.

To Postcard, Edition Cards are cards with fixed customization images — Postcard doesn't need to know these images are Looks/Skins.

### Data Model

```
edition_card (in whimsicard, for discovery/display)
  token
  skin_token
  card_theme_token
  availability: enum { NOT_AVAILABLE, AVAILABLE }

card_skin (enhanced)
  restriction_type: enum { NO_RESTRICTION, PARTNERSHIP_CARD }
```

### Identity Model Tradeoff

Two approaches are under consideration for how Edition Cards are identified in the data model:

| | Approach A: Distinct `card_theme_token` | Approach B: Reuse base token + edition identifier |
|---|---|---|
| **How it works** | Edition Card gets its own token (e.g., `CT_PINK_ARTIST_COLLAB`) | Stores base token (`CT_PINK`) + new `edition_card_token` field |
| **Order tracking** | Simple: `WHERE card_theme_token = 'CT_PINK_ARTIST_COLLAB'` | Composite: requires checking two fields |
| **Re-orders** | Edition token inherited naturally | Must copy edition identifier separately |
| **Downstream impact** | Translation layer needed at Banklin boundary — banking stack must only receive base token | Zero changes to downstream systems |
| **Schema changes** | No new columns needed | New column on `card_customizations` (Vitess migration) |

> See [Tradeoffs: Edition Card Identity Model](https://docs.google.com/document/d/1j2-zgo6heu-xwx-mK8SsHIiZ2WegpSk_MJnu76Hhm4s/edit?tab=t.lri172eebh6y) for the full boundary-point analysis.

### PDP Data

PDP content for Edition Cards is stored in **config files in Whimsicard code**, keyed off the payment device token. This centralizes presentation logic in one service, supports translations via source-stored strings, and allows fast iteration without DB changes.

---

## Relationship to Tags

Edition Cards and Tags are distinct product types that coexist in the Grid:

| | Edition Card | Tag |
|---|---|---|
| **What it is** | Card theme + exclusive skin | Physical NFC payment device (wand, mini card, heart) |
| **Ordering prerequisite** | Standard card ordering eligibility | Must have active card + active sponsorship (if applicable) |
| **Customizable?** | No | No |
| **Availability states** | TEASER → COMING_SOON → NEW → AVAILABLE → OUT_OF_STOCK | Same |
| **PDP** | Own PDP (P1 priority) | Own PDP (P0 priority) |
| **Grid presence** | First-class citizen, skin visible | First-class citizen |

Both follow the same drop culture model: limited editions create scarcity, sold-out and coming-soon states remain visible to build demand and virality.

---

## Open Questions

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | Different pricing for Edition Cards vs. base card theme? | TBD | 🔴 Open |
| 2 | Which identity model approach (A vs B) for Edition Cards? | Server Eng | 🔴 Open |
| 3 | Will Edition Cards support cashtag + skin or drawing + skin customization in the future? (Out of scope for Spring Release) | Anna / John Kang | 🔴 Open — raised by johnkang 2/19 |
| 4 | Can card theme PDPs be descoped for launch? (Edition Card + Tag PDPs are staffed; regular card theme PDPs require additional backend work) | Server Eng / Product | 🟡 Under discussion — raised by rlo 2/9 |
| 5 | Are Skins officially renamed to Looks? | Design | 🟡 Under discussion |

---

## Contributors

- Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))
- Kyle Vermeer ([@kylev](https://sq-block.slack.com/team/kylev))
- Rachel Lo ([@rlo](https://sq-block.slack.com/team/rlo))
- John Kang ([@johnkang](https://sq-block.slack.com/team/johnkang))
- Aleks Gryczon ([@aleksg](https://sq-block.slack.com/team/aleksg))
- Ankur Vashi ([@avashi](https://sq-block.slack.com/team/avashi))
- Lori Choy ([@lorichoy](https://sq-block.slack.com/team/lorichoy))
