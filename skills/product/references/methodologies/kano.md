# Kano Model

> A framework for categorizing features by how they affect user satisfaction — to avoid building the wrong kind of feature at the wrong time.

**Origin:** Noriaki Kano, 1984. Originally a quality management framework, adopted by product teams as a prioritization and positioning tool.

---

## The core idea

Not all features affect satisfaction in the same way. Kano identifies three categories that matter in practice:

| Category | When absent | When present | Shape |
|----------|-------------|--------------|-------|
| **Must-have** (Basic) | Strong dissatisfaction | Neutral — users don't notice | Threshold |
| **Performance** (Linear) | Dissatisfaction | Satisfaction, proportional to quality | Linear |
| **Delighter** (Excitement) | Neutral — users don't miss it | Delight and surprise | Non-linear |

**Must-haves** are the price of entry. Users won't say they want them (they take them for granted), but they'll leave if they're missing. RICE will undervalue them because reach × impact misses the threshold effect.

**Performance features** scale: the better you do them, the more satisfied users are. Speed, reliability, price, comprehensiveness. Users can articulate these as wants.

**Delighters** are features users didn't know they needed. They create strong satisfaction when present, zero dissatisfaction when absent. First-mover advantage is high — but delighters commoditize over time.

---

## The commoditization curve

Features don't stay in their category. Over time:

```
Delighter → Performance → Must-have
```

What delighted users in 2015 (push notifications, real-time sync) is now a must-have. If you're competing in a mature market, your competitor's past delighters are your must-haves. You need new delighters to win — table stakes to stay in the game.

This is why BTD (Below / Table Stakes / Differentiate) is Kano applied to competitive strategy: T = must-have + performance, D = delighter.

---

## How to use it in practice

**As a prioritization lens (before RICE):**
- Identify any must-haves in your backlog first — they need to ship regardless of RICE score, because their absence is a disqualifier
- Performance features are your RICE candidates — score and rank them
- Delighters are bets — validate with BTD (are you choosing to differentiate here?) before investing

**As an interview tool:**
When scoping a feature in a case, ask: *"Is this a must-have, a performance feature, or a delighter for the target segment?"* This changes the recommendation:
- Must-have → ship it, but don't over-invest (users won't reward you for it)
- Performance → invest proportionally to the gap with competitors
- Delighter → invest here if it's your differentiation axis (BTD: D)

**How to identify category in user research:**
Use the Kano question pair:
- *"How would you feel if this feature were present?"*
- *"How would you feel if this feature were absent?"*

| If present: satisfied / If absent: dissatisfied → **Performance**
| If present: neutral / If absent: dissatisfied → **Must-have**
| If present: delighted / If absent: neutral → **Delighter**

---

## In practice — Alan case example

| Feature | Kano category | Implication |
|---------|--------------|-------------|
| 2FA | Must-have | Ship it — not shipping is a trust disqualifier. Don't over-invest. |
| Invoice parsing | Performance | The better it works, the more satisfied users are. Worth investing heavily. |
| Reimbursement journal | Performance → must-have | For frequent claimants, the absence is already a pain. Heading toward must-have. |
| Alan card | Delighter | No other health insurer offers this. Strong differentiation bet — if Alan chooses D here. |

---

## Common traps

- **Over-investing in must-haves** — you can spend a quarter perfecting a must-have and move satisfaction from 0 to 0. Users expected it. The trap is mistaking "users complained about it" for "users will reward us for fixing it."
- **Building delighters when must-haves are broken** — delighters on top of broken basics create a bad product with a shiny feature. Fix the floor before raising the ceiling.
- **Ignoring commoditization** — a performance feature that was your competitive advantage 18 months ago may now be a must-have. Audit your "differentiators" regularly.
- **Applying Kano without segmenting** — a feature can be a must-have for one segment and a delighter for another. Always specify which users you're modeling.

---

## Related methodologies

- [BTD](btd.md) — Kano is the analytical lens; BTD is the strategic choice. Kano tells you what category a feature is; BTD tells you what you choose to do about it.
- [RICE](rice.md) — run Kano first to flag must-haves (ship regardless of score) and delighters (validate before scoring)
- [JTBD](JTBD/) — Kano categories are segment-specific. You need a clear job to know what "must-have" means for your target user.

---

_Last updated: 2026-05-12_
