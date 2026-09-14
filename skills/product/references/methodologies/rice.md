# Prioritization

> Frameworks and principles for deciding what to work on first.

---

## The core challenge

Prioritization fails predictably in the same ways:
- Working on what's interesting or exciting instead of what has impact
- Optimizing for the ideas you'd use yourself (proximity bias)
- Ignoring effort, so high-cost ideas are overweighted
- Not being honest about confidence — treating guesses as data

---

## RICE scoring

**Source:** Sean McBride, Intercom (2018)

RICE is a scoring system to rank project ideas on a single, comparable metric. Useful when you have a long backlog and need a forcing function to cut through bias.

**Formula:** `(Reach × Impact × Confidence) / Effort`

| Factor | What it measures | How to estimate |
|--------|-----------------|-----------------|
| **Reach** | How many users affected per time period | Use real data (e.g. funnel volume, MAU). Unit: people/quarter |
| **Impact** | How much will it move the needle per person | Multiple choice: 3 (massive) / 2 (high) / 1 (medium) / 0.5 (low) / 0.25 (minimal) |
| **Confidence** | How much do you trust your estimates | High = 100% / Medium = 80% / Low = 50% |
| **Effort** | Total person-months across the whole team | Round numbers. Min 0.5. Include design + eng + PM |

**Output:** a number representing total impact per unit of time worked. Sort descending.

### How to use it well

- Don't treat the score as gospel — use it to surface surprises ("why is this so low?") and spot bias ("I was overweighting this because it's my pet idea")
- Once sorted, re-examine: some projects are dependencies, some are table stakes for a customer segment, some need to go first for strategic reasons. These are valid — but name them explicitly rather than quietly inflating a score
- The goal of the framework is not accuracy. It's **calibration** — forcing you to think through the same factors consistently across all ideas

### Real-world caveats

RICE is a good introduction to structured prioritization but shows its limits at scale:
- **False precision:** scores feel objective but rest on subjective impact estimates. A score of 42 vs 38 is not a meaningful difference
- **Confidence collapses nuance:** 80% for "somewhat confident" is a blunt instrument — there are many ways to be uncertain
- **Effort is often systematically underestimated** — RICE doesn't correct for this, it just encodes the bias
- **Strategic value is invisible in the formula** — a bet on a new market or a key partnership doesn't score well on RICE even if it's the right call

Use RICE as a **thinking tool**, not a ranking machine. The output is a conversation starter.

---

## Prioritization principles

- Score to think, not to decide — the framework surfaces implicit assumptions, it doesn't replace judgment
- Before scoring anything, be explicit about your goal: what metric are you trying to move?
- The best prioritization question: *"If we could only ship one thing this quarter, what would change the most for users?"*
- Impact estimates without data are still estimates — confidence scoring is how you mark that honestly

---

## Related methodologies

- [JTBD](JTBD/) — formulating the user job before scoring ensures you're comparing things that compete for the same outcome
- [OKRs](Eagle/methodologies/okrs.md) — the metric you're trying to move (from OKRs) should anchor your Reach and Impact estimates
- [BTD](btd.md) — use BTD upstream of RICE to decide *what kind of bet you're making* on each dimension (differentiate vs. table stakes) before scoring. RICE ranks within a strategy; BTD defines the strategy.

---

_Last updated: 2026-05-06_
