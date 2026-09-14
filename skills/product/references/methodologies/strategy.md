# Product Vision & Strategy

> Strategy is not a goal. It's a set of choices about where you'll play and how you'll win — choices that rule everything else out.

Sources: Richard Rumelt (*Good Strategy Bad Strategy*), Roger Martin (*Playing to Win*), Marty Cagan (SVPG)

---

## Vision vs Strategy — The critical distinction

Most teams confuse these. They're different in time horizon, purpose, and how often they change.

| | Vision | Strategy | OKRs | Roadmap |
|--|--------|----------|------|---------|
| **Question** | Where are we going? | How do we get there? | What will we measure? | What will we build? |
| **Horizon** | 2–5 years | 1–2 years | Quarterly | Quarterly / 6 months |
| **Stability** | Stable | Shifts yearly | Resets each cycle | Changes frequently |
| **Directs** | Strategy | Roadmap + bets | Team priorities | Engineers |

A common failure: teams build a roadmap without a strategy, or a strategy without a vision. Each layer without the one above it drifts.

---

## Product Vision

### What it is

A product vision describes **the future you're creating** — not what you're building, but what the world looks like when you've succeeded.

Good vision is:
- **Inspiring** — it motivates the team and aligns stakeholders
- **Specific enough** to guide decisions and trade-offs
- **Stable enough** to survive quarterly changes

Bad vision is:
- A mission statement ("Empowering users to...")
- A tagline
- A list of features
- Compatible with every possible roadmap — a vision that rules nothing out guides nothing

**The test:** does this vision exclude options? If your vision is compatible with anything, it's not a vision.

---

### Prescriptive vs Reactive Vision

| Type | Source | Risk |
|------|--------|------|
| **Reactive** | Extrapolated from what users say they want | Follows the market, never leads it |
| **Prescriptive** | Conviction about where the industry is going, before users articulate it | Out of touch with real needs if not grounded in research |

The best visions combine both: deep user understanding **plus** a strong point of view on where the industry is heading. Users told Apple they wanted a better MP3 player. The vision was: music, phone, internet — one device.

---

### Key questions for writing a vision

- What does the world look like for our users in 3 years if we succeed?
- What problem will have disappeared from their lives?
- Why does this future matter — and why now?
- Does this vision exclude options? Does it say something we're *not* doing?

---

## Product Strategy

### Rumelt — Good Strategy / Bad Strategy

Richard Rumelt's diagnosis of why most strategy is bad — and what good strategy actually looks like.

> "Bad strategy is not the same as no strategy. It's an active choice to avoid making hard decisions."

**Bad strategy looks like:**
- A list of goals dressed up as strategy ("We will grow 30% and become market leader")
- Vague aspirations with no guiding logic
- Strategic objectives that don't address the actual obstacle
- Fluff — buzzword-heavy language that says nothing

**The Kernel of Good Strategy — three elements, always:**

```
1. Diagnosis       → What is the fundamental challenge? Name it precisely.
2. Guiding Policy  → What is your approach to overcoming it? (This rules out options.)
3. Coherent Actions → The set of moves that implement the policy and reinforce each other.
```

The diagnosis is the most underused element. Teams skip straight to "here's what we'll do" without naming the obstacle clearly. Without a diagnosis, your guiding policy is just a preference.

**Example:**
- **Diagnosis:** Our target segment (freelancers) abandons invoicing tools because the context switch out of their existing workflow is too costly.
- **Guiding Policy:** Reduce workflow friction to the point of invisibility — build deeply into the tools they already live in.
- **Coherent Actions:** Native Gmail integration. One-click payment from email. No-login client experience. Deliberately skip advanced accounting features that serve a different segment.

---

### Roger Martin — Playing to Win

A complementary framework focused on market choices. The cascade flows top to bottom — each level constrains the next.

```
1. Winning Aspiration  → What does winning mean for you and your users?
2. Where to Play       → Which segment, market, geography, channel?
3. How to Win          → What's your competitive advantage in that choice?
4. Capabilities        → What must you be great at to win that way?
5. Management Systems  → How do you build and sustain those capabilities?
```

**The critical insight:** Where to Play and How to Win are inseparable. A "How to Win" without a clear "Where to Play" is just a list of intentions. Most product teams skip straight to Capabilities ("we need to be great at X") without ever locking Where to Play and How to Win. That's execution without strategy.

**The test:** Can you say *where you're choosing not to play* — and why? If not, you haven't made a strategic choice, you've made a list.

---

## BTD — The bridge between strategy and roadmap

BTD (Below / Table Stakes / Differentiate) is the operational translation of strategy into product decisions.

Once strategy defines *where you play and how you win*, BTD forces an explicit answer for each product dimension: are we differentiating here, or accepting table stakes?

**The sequence:** Strategy defines what D must be. BTD operationalizes it.

→ See `btd.md` for the full framework.

---

## Anti-patterns in strategy

- **Goals disguised as strategy** — "We want to be the leader in X" is not a strategy
- **Strategy without diagnosis** — knowing what you'll do without naming the obstacle
- **Copying competitor moves** — "They're doing X, so we should too" inverts the logic. Strategy is about winning where you play, not matching moves
- **Stable strategy in a shifting market** — strategy should shift yearly as the diagnosis shifts
- **Vision detached from research** — a prescriptive vision that ignores user evidence is a bet, not a strategy

---

## Key questions

- What is the fundamental challenge we're solving? (Rumelt: diagnosis)
- Where are we choosing to play — and who are we explicitly *not* serving?
- Why will we win in that space? What makes our approach non-obvious?
- Does our guiding policy exclude options? If not, it's not a policy.
- What must be true for this vision to be wrong?

---

## Resources

- **"Good Strategy Bad Strategy"** — Richard Rumelt (book). The clearest definition of what strategy actually is. Read the first 3 chapters minimum.
- **"Playing to Win"** — Roger Martin (book). The cascade framework. Dense but precise. Chapters 2–3 are the core.
- **"Product Vision vs. Product Strategy"** — Marty Cagan, SVPG blog. 15-min read. Essential for understanding the hierarchy.

---

*Last updated: 2026-05-07*
