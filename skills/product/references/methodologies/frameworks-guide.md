# Framework Guide — When to Use What

> One entry per framework: the trigger condition, the problem it solves, and when not to reach for it.
> This is a navigation tool. Each entry links to the full methodology file where one exists.

---

## PM — Discovery & Research

**JTBD — Jobs to Be Done** | [→ file](JTBD/)
Use when: you need to understand *why* someone would switch to or adopt a product. The core question is "what progress is the user trying to make?" Essential before defining a new product or repositioning an existing one.
Skip when: you already have strong behavioral data and the question is execution, not understanding.

**Switching Interviews** | [→ file](switching-interviews.md)
Use when: you need to diagnose *why* customers churn, why win rates are declining in a segment, or what job a product is actually being hired for. The method reconstructs the causal story of a switching decision — triggering event, competing forces, and moment of commitment. Run this before jumping to solutions; it surfaces the job, not the fix.
Skip when: you already have strong causal evidence from behavioral data. Also not the right tool for NPS-style satisfaction research — it's diagnostic, not evaluative.
Watch out: usage data right before churn tells you nothing about whether the job was done. High activity + high churn = the product was hired for the wrong job, or failed to deliver on the right one.

**Design Thinking** | [→ file](Design%20thinking.md)
Use when: the problem space is ambiguous and the solution is unknown. Best for workshops, early-stage discovery, or cross-functional alignment on a problem definition.
Skip when: the problem is already well-defined — design thinking at that stage introduces scope creep.

**Continuous Discovery (Teresa Torres)** | [→ file](discovery.md)
Use when: you want to build a sustainable habit of weekly user contact tied directly to opportunity mapping. Works best as an operating rhythm, not a one-time exercise.
Skip when: you're pre-PMF and discovery needs to be intensive and unstructured, not rhythmic.

**User Research** | [→ file](user_research.md)
Use when: you need primary data — interviews, usability tests, surveys — to validate or invalidate a hypothesis. Reach for it before any significant bet, not after.
Skip when: you're looking to confirm what you already believe. Research that starts with a hypothesis and ends with "we were right" was designed wrong.

---

## PM — Prioritization

**Kano Model** | [→ file](kano.md)
Use when: starting a prioritization exercise — flag must-haves (ship regardless of score) and identify delighters (validate as differentiation bets) before scoring anything. Reduces noise in RICE by removing features that shouldn't compete on the same scale.

**AARRR — Acquisition → Activation → Retention → Referral → Revenue** | [→ file](aarrr.md)
Use when: you need to identify which stage of the funnel is the bottleneck before looking at individual features. Run this first — it scopes the entire prioritization exercise.
Skip when: your product isn't growth-stage or when the funnel metaphor doesn't fit (e.g. pure B2B enterprise with no self-serve).
Watch out: in B2B, Acquisition means winning deals, not individual signups — adapt the "A" stage accordingly.

**BTD — Below / Table Stakes / Differentiate** | [→ file](btd.md)
Use when: you've identified the bottleneck stage (AARRR) and need to decide what kind of bet to make on each candidate idea — what to be great at vs. what to merely cover.
Skip when: you're doing execution-level prioritization within an already-defined strategy. BTD is a strategy tool, not a sprint planning tool.

**RICE — Reach × Impact × Confidence / Effort** | [→ file](rice.md)
Use when: you have a scoped list of competing ideas (post-AARRR, post-BTD) and need a consistent, defensible way to rank them. Forces you to make assumptions explicit.
Skip when: you have fewer than 3 ideas (overkill) or when you're making a binary yes/no call on a single feature.
Watch out: RICE scores are as good as the estimates behind them. A high score on bad inputs is noise, not signal.

---

## PM — Metrics & Diagnostics

**North Star & Metrics Framework** | [→ file](north-star-metrics.md)
Use when: defining what your crew optimizes for, setting OKRs, or evaluating whether a proposed metric is meaningful. The core question: is this an outcome metric (user achieved their goal) or just an adoption metric (user showed up)?
Skip when: you're pre-PMF — forcing a North Star too early creates false precision on a direction that hasn't been validated yet.

**Metric Investigation — Internal / External Causes** | [→ file](metric-investigation.md)
Use when: a metric moves unexpectedly and you need to diagnose why. Standard structure for PM interviews ("DAU dropped 20%, walk me through it") and real incidents. Always validate the data before investigating causes.
Skip when: the cause is already obvious and confirmed — the framework is a search tool, not a ritual.

**Kano Model** | [→ file](kano.md)
Use when: you need to categorize features before prioritizing — distinguishing must-haves (ship regardless, don't over-invest) from performance features (RICE candidates) from delighters (strategic bets). Run before RICE to flag features that should bypass scoring.
Skip when: you don't have enough user proximity to assign categories with confidence — Kano without user data is guesswork.

---

## PM — Strategy & Goals

**OKRs — Objectives & Key Results** | [→ file](Eagle/methodologies/okrs.md)
Use when: you need to align a team around a measurable outcome for a time-bound period. Most useful at the team or crew level, less so at the individual level.
Skip when: the environment is too uncertain to commit to key results — forcing OKRs on a pre-PMF team creates false precision.

**Strategy** | [→ file](strategy.md)
Use when: you need to make an explicit choice about where to play and how to win. Strategy is a set of bets, not a list of goals.
Skip when: the question is "what should we build next?" — that's prioritization, not strategy.

**Competitive Analysis** | [→ file](competitive-analysis.md)
Use when: you're entering a new space, repositioning, or evaluating why you're losing deals. Grounds strategy in market reality.
Skip when: you're early in discovery — competitive analysis too early anchors you on existing solutions rather than user jobs.

**Roadmap** | [→ file](roadmap.md)
Use when: you need to communicate a sequence of bets to stakeholders over time. A roadmap is a communication artifact, not a contract.
Skip when: you're still in discovery. A roadmap before clarity on what to build creates false confidence and locks in the wrong scope.

**Product Strategy Process** | [→ file](product-strategy-process.md)
Use when: you need a structured process to move from market insight to strategy to roadmap. A meta-framework that sequences the other tools.
Skip when: you need speed over rigor — this is a thorough process, not a quick-alignment tool.

---

## Communication

**Pyramid Principle (Minto / SCQA)**
Use when: writing any document where the reader's time is scarce and the conclusion needs to land first. Structure: Answer → Situation → Complication → Key Question → Supporting arguments. Essential for exec comms, case write-ups, and any async doc that needs to be read in 30 seconds.
Skip when: the audience needs to be taken through the reasoning before the conclusion — e.g. sensitive decisions where the "why" must precede the "what."

**Facilitation** | [→ file](facilitation.md)
Use when: you're running a workshop, alignment session, or any meeting where decisions need to be made by a group rather than by you alone.
Skip when: the decision is yours to make — facilitation in that context is abdication.

---

## Stakeholder

**Influence Mapping (Power / Interest grid)**
Use when: you're starting a new initiative or navigating organizational complexity. Maps stakeholders on two axes — level of power and level of interest — to decide where to invest relationship-building time.
Skip when: the stakeholder set is small and relationships are already clear. Overmapping a simple situation creates bureaucracy.

**RACI — Responsible / Accountable / Consulted / Informed**
Use when: you need to clarify ownership on a cross-functional initiative with multiple contributors. Prevents both decision paralysis (too many accountable) and blind spots (someone critical not consulted).
Skip when: the team is small and roles are obvious. RACI on a 3-person crew is overhead, not clarity.

---

_Last updated: 2026-05-12_
