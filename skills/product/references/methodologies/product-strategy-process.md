# Product Strategy Process

> From research to roadmap — the full sequence for defining a vision, strategy, and roadmap grounded in user and market reality.

---

## The core principle

This process is **not a waterfall**. Vision, strategy, and roadmap coexist at different time horizons and update at different cadences. Research is continuous, not a phase. The layers below don't run sequentially once — they run in parallel, each informing the others over time.

| Layer | Time horizon | Cadence |
|-------|-------------|---------|
| Research | Ongoing | Weekly (Torres: minimum 1 user per week) |
| Vision | 2–5 years | Revisit annually, or when market shifts fundamentally |
| Strategy | 1–2 years | Revisit annually, adjust when diagnosis shifts |
| Roadmap | Quarterly | Reviewed every quarter, updated continuously |

---

## Layer 1 — Understand

**What you're building:** a rich, evidence-based picture of user needs and market dynamics.

### Tools

| Framework | File | Question answered |
|-----------|------|------------------|
| JTBD interviews | [JTBD/](JTBD/) | What job are users hiring a product to do? What are the push/pull forces around switching? |
| Story-based interviews (Torres) | [user_research.md](user_research.md) | What is the user's actual lived experience — not what they say they'd do, but what they did? |
| Competitive analysis | [competitive-analysis.md](competitive-analysis.md) | Where do current solutions systematically fail specific segments? Where is the market going? |

### Key output

A map of **tensions** — places where users have a real, frequent problem and current solutions fall short. Not a list of feature requests. Tensions.

### Anti-patterns to avoid

- Treating research as a project with a start and end date
- Asking users what they want instead of understanding what they do
- Analyzing the competitive landscape as a feature matrix

---

## Layer 2 — Synthesize

**What you're building:** structure in the problem space, before generating any solutions.

### Tools

| Framework | File | Question answered |
|-----------|------|------------------|
| Opportunity Solution Tree (Torres) | [user_research.md](user_research.md) | What are the opportunities — user needs, pain points, desires — that sit between the desired outcome and potential solutions? |
| Segmentation | [user_research.md](user_research.md) | For which segment is the problem most acute? Who is most underserved by what exists? |
| Incumbent Gap Analysis | [competitive-analysis.md](competitive-analysis.md) | Where do incumbents structurally fail our target segment — and why are they slow to fix it? |

### Key output

A clear view of **which opportunity, for which segment**, is the most compelling bet. This is the input to strategy — not a feature list, not a solution, an opportunity.

### The most common skip

Teams jump from research directly to solutions. The synthesis layer is skipped, and the roadmap ends up being a list of feature requests with no strategic coherence. The OST exists precisely to prevent this.

---

## Layer 3 — Decide

**What you're building:** the vision, strategy, and positioning choices that will direct the roadmap.

### Tools

| Framework | File | Question answered |
|-----------|------|------------------|
| Product Vision | [strategy.md](strategy.md) | What future are we creating? What does the world look like when we've succeeded? |
| Rumelt's Kernel | [strategy.md](strategy.md) | What is the fundamental challenge (diagnosis)? What's our guiding policy? What coherent actions follow? |
| Roger Martin's Cascade | [strategy.md](strategy.md) | Where do we play? How do we win in that space? |
| BTD | [btd.md](btd.md) | On which dimensions do we differentiate? What do we consciously accept as table stakes or below? |

### Key output

- A written vision statement (2–5 year horizon)
- A written strategy: diagnosis + guiding policy + coherent actions
- Explicit BTD choices for the core product dimensions

### The critical tension

Vision can be **reactive** (derived from research) or **prescriptive** (based on a conviction about where the market is going). Great visions are usually both. Research grounds you in real user needs; prescriptive thinking lets you lead rather than follow.

Don't build a vision purely from user research. Users tell you what they want today — not what they'll need in 3 years when the market shifts.

---

## Layer 4 — Execute

**What you're building:** the concrete bets, sequenced into a roadmap, connected to measurable outcomes.

### Tools

| Framework | File | Question answered |
|-----------|------|------------------|
| OKRs | [okrs.md](Eagle/methodologies/okrs.md) | What outcomes will we drive this quarter? How will we know if we're winning? |
| Now / Next / Later | [roadmap.md](roadmap.md) | What are we building now, what's next, and what's in the vision but not yet sequenced? |
| RICE | [rice.md](rice.md) | Within a given horizon, which bet has the highest impact per effort? |

### Key output

An outcome-based roadmap with a clear "Now" (committed this quarter), a directional "Next," and a thematic "Later" — each connected to strategy and OKRs.

---

## The full picture

```
[RESEARCH — continuous]
JTBD interviews + Story-based interviews + Competitive analysis
                        ↓
[SYNTHESIS]
Opportunity Solution Tree → Segmentation → Incumbent Gap Analysis
                        ↓
[STRATEGY]
Vision (where we're going)
Rumelt Kernel (diagnosis → guiding policy → coherent actions)
Martin Cascade (where to play → how to win → capabilities)
BTD (differentiate vs table stakes vs below)
                        ↓
[EXECUTION]
OKRs → Now / Next / Later roadmap → RICE (within-horizon ranking)
```

---

## How to use this process in practice

**Starting from scratch (new product or strategic reset):**
1. Run 10–15 JTBD interviews with your target segment
2. Layer in competitive analysis — read 3-star reviews, identify incumbent gaps
3. Build the OST — map the opportunity space before touching solutions
4. Choose the segment and opportunity to bet on
5. Write the strategy kernel (diagnosis first — this is the hardest step)
6. Derive BTD choices from the strategy
7. Set OKRs for the quarter
8. Fill Now / Next / Later

**Ongoing (established product):**
- Research is continuous — 1 user per week minimum
- Strategy is revisited annually or when the diagnosis shifts
- Roadmap is reviewed quarterly, updated continuously
- BTD choices are revisited when new competitive data changes the landscape

---

## Key questions — the full process

| Layer | Forcing question |
|-------|-----------------|
| Research | Are we talking to users whose experience would change our decisions — or just confirming what we already believe? |
| Synthesis | Have we structured the problem space before jumping to solutions? |
| Strategy | Can we name the fundamental challenge we're overcoming? Can we say what we're *not* doing? |
| Execution | Does each roadmap item connect to an OKR and a strategic bet? |

---

## Related files

- [discovery.md](discovery.md) — continuous discovery principles (Cagan + Constable)
- [user_research.md](user_research.md) — Torres, OST, segmentation
- [JTBD/](JTBD/) — jobs to be done frameworks
- [competitive-analysis.md](competitive-analysis.md) — market and competitive analysis
- [strategy.md](strategy.md) — vision, Rumelt, Roger Martin
- [btd.md](btd.md) — positioning decisions
- [okrs.md](Eagle/methodologies/okrs.md) — outcome setting
- [roadmap.md](roadmap.md) — Now/Next/Later
- [rice.md](rice.md) — within-horizon prioritization

---

*Last updated: 2026-05-07*
