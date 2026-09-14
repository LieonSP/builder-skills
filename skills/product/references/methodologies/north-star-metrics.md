# North Star & Metrics Framework

> A framework for defining the one metric that best captures the value your product delivers to users — and building a coherent metrics system around it.

**Origin:** Popularized by Amplitude (John Cutler's *North Star Playbook*, 2018). Related to Sean Ellis's "one metric that matters" and Shreyas Doshi's work on metric types.

---

## The core idea

A North Star Metric (NSM) is the single metric that best captures the value your product delivers to users *at scale*. It sits between user value and business outcomes: if it goes up, users are getting more value, and business results will follow.

The NSM is not a business metric (revenue, churn). It's not a vanity metric (signups, pageviews). It's a measure of whether users are achieving the outcome they came for.

---

## The three metric types

Before defining a North Star, you need to distinguish between three types:

| Type | What it measures | The trap |
|------|-----------------|----------|
| **Output metric** | What the team produced — features shipped, experiments run | Confuses activity with results |
| **Adoption metric** | Whether users are using something — DAU, logins, feature usage | Can go up while the user is still suffering |
| **Outcome metric** | Whether the user achieved the result they came for | The only valid North Star candidate |

**The test:** *"Can this metric go up while the user continues to suffer?"* If yes — it's not a North Star. It's an adoption metric at best.

Example: "number of invoices created" goes up the moment you acquire any new user, regardless of product quality. "Invoices paid on time" cannot go up unless the product is actually working for the user.

---

## Criteria for a good North Star Metric

A strong NSM satisfies all five:

1. **Captures user value** — it measures an outcome the user actually cares about, not a proxy
2. **Leading indicator of business success** — it predicts retention, revenue, and growth before they show up in financial metrics
3. **Actionable by the product team** — the team can directly influence it through product decisions
4. **Understandable** — anyone in the company can explain what it means and why it matters
5. **Not gameable** — you can't inflate it without actually delivering value (e.g. "active users" is gameable; "users who completed their core job" is harder to fake)

---

## Supporting metrics (input metrics)

The NSM alone doesn't tell you *why* it's moving. You need 3–5 input metrics that are the leading levers of the North Star.

Structure:
```
North Star Metric
  ├── Input metric 1 (e.g. activation rate)
  ├── Input metric 2 (e.g. core action completion rate)
  ├── Input metric 3 (e.g. D30 retention)
  └── Input metric 4 (e.g. support ticket deflection rate)
```

When the NSM drops, you look at input metrics to find the lever that moved. When you prioritize features, you should be able to state which input metric each idea addresses.

---

## Workaround as NSM signal

When users systematically work around your product to accomplish their job (export to Excel, duplicate entries, use a competitor for one step), the workaround is a signal that the NSM should measure the *absence of that workaround*. The product hasn't solved the job — it's just been tolerated.

*"Proportion of users who complete [core job] without leaving the product"* is often a stronger NSM than a pure usage metric.

---

## Common mistakes

- **Choosing a business metric as NSM** — revenue and churn are lagging indicators. By the time they move, the product problem is months old.
- **Choosing an adoption metric** — logins, DAU, and feature usage don't prove value delivery. They prove presence, not impact.
- **Multiple North Stars** — two NSMs create prioritization conflicts. If you genuinely have two, you probably have two products or two crews. Split accordingly.
- **Setting the NSM and never revisiting it** — as the product matures and the core job changes, the NSM should evolve. Review annually at minimum.
- **Picking a metric you can't currently measure** — an ideal NSM you can't instrument is worse than a good-enough NSM you can track today.

---

## In practice — examples

| Product | North Star candidate | Why |
|---------|---------------------|-----|
| Slack | Messages sent per active user per week | Captures habit formation and collaboration value, not just presence |
| Airbnb | Nights booked | Captures both sides of the marketplace in one number |
| Pennylane | Invoices paid on time without leaving the product | Measures the core job (cash flow management) without counting workarounds |
| Alan | Active members (claimed or engaged in past 90 days) | Captures insurance value actually used, not just coverage purchased |

---

## Related methodologies

- [AARRR](aarrr.md) — AARRR maps the funnel; the NSM usually lives at the Retention or Activation stage
- [OKRs](Eagle/methodologies/okrs.md) — the NSM is the anchor for crew-level OKRs; key results should be input metrics
- [Discovery](discovery.md) — when the NSM drops, discovery is how you understand why

---

_Last updated: 2026-05-12_
