# Edition Cards

> **Status:** Review by Anna on 3/8
> **Author:** Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))  
> **Source doc:** [Customization Domain Exploration](https://docs.google.com/document/d/1j2-zgo6heu-xwx-mK8SsHIiZ2WegpSk_MJnu76Hhm4s/edit)  
> **Slack:** [#proj-mint](https://sq-block.slack.com/archives/C08QQ6KQ8PJ)

---

## What Is an Edition Card?

An Edition Card is a new concept for card customization; the goal of which is to attract net new customers and re-engage existing customers by making cool cards easier to accces. Edition Cards are the primary surface for partnership designs that could be limited in volume or exclusive. 

More specifically, an Edition Card is a **card theme + skin/look** where the skil design is 1:1 with the card theme. To the end customer, an Edition Card looks and feels like its own unique product — even if, on the backend, it's a base card theme with an immutable customization image.

### Glossary

| Term | Definition |
|------|-----------|
| **SKU / Theme** | The physical piece of plastic (e.g., Pink Card, Holo Card) |
| **Skin or Look** | A design applied on top of a card theme |
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
| 2 | Stated inventory possible, but not required (e.g., 10,000 RuPaul cards) | Primarily for marketing. Creates scarcity narrative. We will **not** live-track inventory in eng systems or auto-cutoff after N orders initially, unless this is easy to configure. The true inventory limitation is the underlying card theme stock (e.g., pink cards). Marketing can optionally turn off an Edition Card by directive. |
| 3 | Edition Card skins are exclusive | The RuPaul skin should **not** appear in the standard skin / looks selection interface. Customers scrolling through skins on a basic card will not randomly see skins that appear on Edition Cards. This keeps Edition Cards feeling special and distinct. |
| 4 | First-class citizen in Grid | Edition Cards appear in the Grid alongside existing themes and Tags. In the Grid, Edition Cards are displayed with their skin visible, while other cards remain plain — clarifying what the user is ordering "as is." |
| 5 | Own Product Detail Page (PDP) | Edition cards have their own PDP pages. Each Edition Card gets its own marketing content, hero imagery, and description. PDP pages have their own deep links so they can be used in marketing. PDP priority: Tags P0, Edition Cards P1, Non-Edition Card Theming P2. |
| 6 | Immutable | Edition Cards are ordered as-is. The customer cannot further customize an Edition Card with drawings, stamps, or alternative skins. This simplifies the initial launch scope and preserves the integrity of the partnership design. |
| 7 | Multiple Edition Cards live at once | The system supports multiple active Edition Cards simultaneously. |
| 8 | Reportable | We can report on Edition Card orders specifically (e.g., how many RuPaul cards ordered) as well as by underlying theme (e.g., all pink cards, including RuPaul). RuPaul cards count as pink cards (mostly so we can manage hardware inventory operationally), but other pink cards do not count as RuPaul cards. |
| 9 | Removable / sell-out logic | Edition Cards can be removed from the customer experience (sold out, retired, etc.). When removed, they should not appear in re-order scenarios. This applies at all levels: SKUs, Skins, and Edition Cards should all have filtering logic for re-orders. A customer must select a new card type when they re-order (e.g. we will not auto-reissue the design upon expiry) |
| 10 | Pricing is independent of underlying card theme | Edition Cards can be priced independently of the underlying card theme. For example, the RuPaul card could be offered at $5 while the Pink Card could be offered for free |

### Availability States

Edition Cards follow the same availability state model as Tags. It's not required that we apply one of these states each time a new Edition Card drops, but it should be available and possible:

| State | Description | Customer Experience |
|-------|-------------|-------------------|
| **TEASER** | Well before release date | Customer sees a teaser — builds intrigue, no details yet |
| **COMING_SOON** | Approaching release date | Customer sees the Edition Card in the Grid with a "Coming Soon" treatment. Can't order, but knows it's dropping soon. Builds hype. |
| **NEW** | Just after release | Customer sees a "New" pill. Can order. |
| **AVAILABLE** | General availability | Standard orderable state |
| **OUT_OF_STOCK** | Sold out or retired | Customer sees the Edition Card in the Grid with a "Sold Out" state. Still visible — mirrors e-comm experiences and drop culture. Drives demand for future drops. |

> **Note:** Edition Card availability depends on the underlying card theme's availability from Postcard. If the base Pink Card is out of stock, the RuPaul Edition Card is also out of stock.

---

## Engineering Architecture (Summary)

> The below was architected prior to the RIF on 2/27. Engineering should cross reference any design documents with the markdown file to ensure consistency in product context and technical design.

> Eng design lives in the [source doc](https://docs.google.com/document/d/1j2-zgo6heu-xwx-mK8SsHIiZ2WegpSk_MJnu76Hhm4s/edit). Engineering should cross reference any design documents with this markdown file to ensure consistency in product context and technical design.

---

## Open Questions

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | Different pricing for Edition Cards vs. base card theme? | TBD | 🔴 Open |
| 2 | Which identity model approach (A vs B) for Edition Cards? | Server Eng | 🔴 Open |
| 3 | Will Edition Cards support cashtag + skin or drawing + skin customization in the future? (Out of scope for Spring Release) | Anna / John Kang | 🔴 Open — raised by johnkang 2/19 |
| 4 | Can card theme PDPs be descoped for launch? (Edition Card + Tag PDPs are staffed; regular card theme PDPs require additional backend work) | Server Eng / Product | 🟡 Under discussion — raised by rlo 2/9 |

---

## Contributors

- Anna Thompson ([@annat](https://sq-block.slack.com/team/annat))
- Kyle Vermeer ([@kylev](https://sq-block.slack.com/team/kylev))
- Rachel Lo ([@rlo](https://sq-block.slack.com/team/rlo))
- John Kang ([@johnkang](https://sq-block.slack.com/team/johnkang))
- Aleks Gryczon ([@aleksg](https://sq-block.slack.com/team/aleksg))
- Ankur Vashi ([@avashi](https://sq-block.slack.com/team/avashi))
- Lori Choy ([@lorichoy](https://sq-block.slack.com/team/lorichoy))
