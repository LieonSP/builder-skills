# Product Roadmap

> A roadmap is not a promise of what will be built. It's a communication tool that shows what bets you're making — and why.

Sources: Janna Bastow (ProductPlan), Marty Cagan (SVPG), Jeff Patton

---

## The problem with feature roadmaps

Most roadmaps are feature lists with dates attached. This creates predictable failures:

- **False commitment** — dates become promises before the team understands what the problem actually requires
- **Output focus** — shipping the feature becomes the goal, not solving the problem
- **Rigidity** — when discovery invalidates an assumption, the team ships anyway to "hit the date"
- **No learning loop** — a roadmap that's fully delivered looks like success even when outcomes don't move

The alternative: **outcome-based roadmaps** — structured around what you're trying to change for users, not what you'll build.

---

## Now / Next / Later

**Origin:** Janna Bastow, co-founder of ProductPlan.

A simple, honest roadmap structure that communicates intent without overpromising.

| Column | Time horizon | What goes here | Level of detail |
|--------|-------------|----------------|-----------------|
| **Now** | This quarter | Active work — you're committed | Specific outcomes + key bets |
| **Next** | Next quarter | Strong candidates — direction set, details TBD | Outcomes, not features |
| **Later** | Beyond | In the vision — not yet sequenced | Problems or themes only |

**The key discipline:** only "Now" has real commitment. "Next" and "Later" are intentionally fuzzy — they communicate direction, not delivery dates.

---

### How to fill it

Write each item as an **outcome or problem**, not a feature:

| Wrong (feature) | Right (outcome / problem) |
|-----------------|--------------------------|
| "Add export to CSV" | "Users can move their data into existing tools without manual work" |
| "Redesign dashboard" | "Users understand their account status without contacting support" |
| "Mobile app" | "Users can complete core tasks without being at a desk" |

If you can't describe an item as an outcome, it means you haven't yet understood the problem behind the feature request.

---

## Connecting roadmap to OKRs

The roadmap serves the OKRs. Each "Now" item should connect to at least one Key Result.

If an item on the roadmap doesn't map to any OKR, ask: why is it on the roadmap?

Exceptions are valid — technical debt, compliance, infrastructure — but they should be **explicit exceptions**, not the default.

```
OKRs (quarterly)
    └── Roadmap "Now" items — bets to move Key Results
            └── Discovery work — validating assumptions before building
                    └── Delivery — building what was validated
```

---

## Stakeholder communication

A roadmap is a communication tool first. Different audiences need different cuts:

| Audience | What they need | How to frame it |
|----------|---------------|-----------------|
| Engineering | What we're building now and next | Detailed Now, rough Next |
| Leadership | What bets we're making and why | Themes + outcomes + strategic rationale |
| Sales / CS | What's coming that helps customers | Problem framing, not features |
| Customers | What problems we're solving | Outcome language only, no dates |

Never show the full technical roadmap to customers or sales without translating it first. Features without context create expectations you'll be held to.

---

## Anti-patterns

- **Gantt chart disguised as a roadmap** — dates on everything = false precision, false accountability
- **Features instead of outcomes** — the team optimizes for shipping, not for impact
- **"Later" as a dumping ground** — if Later is never revisited and never killed, it's not a roadmap, it's a backlog graveyard
- **Roadmap without strategy** — if you can't explain why these items and not others, there's no strategy behind the roadmap
- **Immovable roadmap** — a roadmap that never changes despite new learning is a sign the team isn't doing discovery

---

## Key questions

- Does each "Now" item connect to an OKR or an explicit strategic bet?
- Is the outcome measurable? Will we know if we've succeeded?
- What would have to be true for a "Next" item to move to "Now"?
- When did we last kill something from "Later"? If never, it's not a living document.
- Can each item be written as a problem or outcome — or is it still just a feature?

---

## Related

- [OKRs](Eagle/methodologies/okrs.md) — the outcomes the roadmap is designed to serve
- [Prioritization / RICE](rice.md) — ranking within a horizon
- [BTD](btd.md) — positioning decisions that shape what goes in "Now" vs "Later"
- [Product Strategy Process](product-strategy-process.md) — where the roadmap sits in the full process

---

*Last updated: 2026-05-07*
