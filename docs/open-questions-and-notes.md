# Project Mint — Open Questions, Concerns & Notes

> A running dump of thoughts, open questions, unresolved concerns, and things that need to be addressed across Project Mint. Not a to-do list — a place to make sure nothing falls through the cracks.

---

## Edition Cards

### Definition & Scope
- Edition Card = SKU + Skin (1:1, immutable). Clear for May — but what happens when we want to evolve this? At what point do we allow limited customization (e.g., adding a $cashtag to an Edition Card)?
- "Edition Card" naming abstracts from "partnership" which is good, but do we need clearer internal language for partner-branded vs Cash App brand-generated Edition Cards? They may have different operational needs (approvals, revenue share, marketing constraints)
- Are we confident that the 1:1 skin-to-SKU relationship scales? What if a partner wants the same skin on multiple hardware themes?

### Inventory & Ordering
- Marketing inventory (stated numbers like "10,000 RuPaul cards") vs actual inventory (physical card stock) — the gap between these could create customer confusion if a card is "available" but hardware is out of stock
- Manual cutoff for Edition Cards feels fragile. Who owns the decision to turn off an Edition Card? Marketing? Product? What's the escalation path if we need to kill one fast?
- n=1 ordering limit per Limited Edition SKU — how do we handle edge cases? Customer orders, card is lost in mail, wants to re-order the same Limited Edition. Do they get an exception?
- Re-order scenarios for sold-out Edition Cards: what does the customer see? Just gone from the Grid? Greyed out? "Sold out" badge? This matters for the emotional experience of drops

### Financial & Reporting
- FinPlat SKU mapping is still an open area (flagged by John Kang). This feels like it could be a blocker if not resolved soon — we need clean reporting on Edition Cards vs base themes
- Pricing strategy: defaulting to base hardware price makes sense for acquisition, but do we have flexibility to price Edition Cards differently if a partner deal requires it?
- How do we attribute revenue/cost for Edition Cards? If RuPaul card = Pink Card hardware + RuPaul skin, where does the partner cost sit?

### Backend & Technical
- Kyle's concern about launching multiple new cards through Marqeta + fulfillers being aggressive on timeline — is this resolved? His suggestion of a "pseudo-SKU that only remains in the customization world" felt pragmatic
- Do fulfillers actually need to know about Edition Cards, or is it purely a customization-layer concept? I thought they didn't, but I might be missing something
- Card Risk team (Jingxing Wang) is waiting on PostCard <> MIR integration for card customization model. Is this on track?

---

## Grid & Card Design Library

### Ordering & Discovery
- Grid ordering logic has a MECE problem: "Charms or partnership cards" and "Partnership cards" appear as separate buckets. Need cleaner, mutually exclusive categories
- When does something stop being a "new drop"? Need simple, automated rules for transitioning from "new" → standard placement. Manual maintenance won't scale
- Edition Cards show with skin visible in Grid; regular cards show plain. Does this create a visual hierarchy that makes regular cards feel less appealing? Is that intentional?

### Product Detail Pages (PDP)
- PDP for Edition Cards is P1, PDP for non-Edition Card theming is P2, PDP for Tags is P0. Server eng has staffing for Tags and Edition Cards PDPs, but regular card theme PDPs need additional work (Rachel flagged this as a risk given limited bandwidth). Can we descope card theme PDPs for launch?
- Each Edition Card gets its own PDP — who creates the content? Product? Marketing? Partners? What's the approval flow?

### Card Studio & Skins
- Skins are immutable for May. But the question of $cashtag + skin compatibility came up (John Kang, Feb 19). If a customer can't add their $cashtag to a skinned card, that's a meaningful limitation we should communicate clearly
- "Safe areas" on metal and mood cards that don't allow edge drawing — are these well-documented for the skins team?
- Are Skins officially renamed to "Looks"? (Rachel asked, no clear answer yet)

---

## Physical Cashtags / Tags

### NFC Activation
- Teen Advisory Council research (Feb 2026) surfaced real concerns: unclear positioning (front vs back of phone), "don't move" requirement was hard to maintain, multiple devices failed to activate. If this happens post-purchase, teens said they'd be "annoyed" and "stressed" and may return the device
- Optimal conditions seem to be: tag in packaging, clear NFC target instructions, clear error messaging. Are we confident the launch experience meets this bar?
- What's our plan if activation failure rates are high at launch? Do we have a fallback or support escalation path?

### Drops & GTM
- 3 limited-edition drops (Wand → Mini Card → Heart) starting Spring 2026, then evergreen at Back to School. First drop lands on Spring Releases — are we confident in hardware supply?
- Limited Edition SKUs need hardware designs locked by 2/9 to meet supply schedule. Did we hit this deadline?
- Limited edition could mean differently colored carabiners or ribbons (NOT different form factors). Is this well understood by all teams?
- Teens see high virality potential — influencer seeding, pop-ups, collabs (Love Shack Fancy, Wicked, Disney, Sabrina Carpenter). Are we resourced to execute on this?

### Compliance & Disputes
- Reg E applies to Cashtag orders when a linked instrument is charged (confirmed by Disputes counsel). We need a dispute process before launch — this doesn't exist today even for regular Cash App Card orders via P2P
- Reg E disclosure requirements for virtual/non-physical payment devices: financial institution name, website, and telephone number must be disclosed at the entry point. Celeste Jackson flagged we may not be compliant for existing virtual cards either — this needs to be resolved before Cashtags launch
- What's the dispute flow for a Cashtag that never arrives? Is it goods/services not received (not Reg E) or a processing error (Reg E)?

---

## Staffing & Resourcing Concerns

- Design DRI transitioned from Aleks Gryczon to Lori Choy — is the handoff clean? Are there context gaps?
- Kyle Vermeer went OOO — who is picking up his server eng work? (Jingxing Wang asked this, unclear answer)
- Morgan Collino going on paternity leave — he was driving Grid prototyping. Is there coverage?
- Server eng bandwidth is tight: Tags PDP + Edition Cards PDP are staffed, but regular card theme PDPs are at risk. Do we need to make a hard call on scope?
- Card Risk team needs PostCard <> MIR integration — is there an eng owner driving this?

---

## Tag Activation & Provisioning

- **OTP/2FA on every activation attempt is a non-starter.** Maggie's testing (3/5/2026) surfaced that the Fidesmo provisioning flow may require customers to verify via OTP or SMS for each activation attempt. If a customer fails NFC activation (which teen research already showed is common), they'd have to re-authenticate every time they retry. This will destroy the activation experience — especially for teens. We need to confirm: does each activation attempt require a new OTP? If so, we need to fix this before launch.
- Maggie's tag was provisioned successfully (got confirmation email) but transactions were declined with `STAND_IN_BY_NETWORK` / `DO_NOT_HONOR` — a hard network decline, not a Cash App decline. Her physical card worked fine at the same merchant. Ricky Graziosi is reaching out to Marqeta to investigate. This is concerning — if the network is blocking tag transactions, that's a fundamental issue.
- The Fidesmo "Activate Payment Card" step may be getting skipped or not surfacing properly. Maggie thought she completed activation but may have missed a step. The "activate using your bank's app" flow sends to App Store → Cash App but shows no prompt. The "call bank" option returns a Verizon "call cannot be completed" error. Both paths are broken.
- General concern: the activation flow has too many failure modes (NFC positioning, OTP friction, Fidesmo UX confusion, network declines) and not enough clear error recovery. Each one individually is manageable, but stacked together they create a really frustrating first experience.

---

## Things That Feel Unresolved

- The relationship between Edition Cards and the broader "drops" culture we're building. Are Edition Cards always tied to drops? Can there be an "evergreen" Edition Card?
- How do Edition Cards interact with Cash App for Kids? Kids can order cards — can they order Edition Cards?
- Web team involvement in the release — Ankur was supposed to start the conversation. Did this happen?
- The tension between "Edition Cards are immutable" and the broader vision of card customization. Are we building two parallel systems that will need to converge later?

---

*Last updated: Mar 5, 2026*
