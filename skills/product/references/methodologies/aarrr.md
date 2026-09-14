# AARRR — Pirate Metrics

> A funnel diagnostic framework for identifying which stage of the user journey is the binding constraint on growth.

**Origin:** Dave McClure, 2007 — [Original slide deck](https://www.slideshare.net/dmc500hats/startup-metrics-for-pirates-long-version) — coined at 500 Startups, widely adopted in growth and product circles.

---

## The core idea

Before prioritizing features, ask: *where in the funnel are we losing people?* AARRR maps the user journey into five stages and forces you to identify the bottleneck before deciding what to build.

| Stage | Question | What it measures |
|-------|----------|-----------------|
| **A — Acquisition** | How do users find us? | Traffic, leads, new signups — top of funnel |
| **A — Activation** | Do they have a good first experience? | First meaningful action, "aha moment" |
| **R — Retention** | Do they come back? | Repeat usage, D7/D30/D90 active rates |
| **R — Referral** | Do they tell others? | NPS, word-of-mouth, viral coefficient |
| **R — Revenue** | Do they pay? | Conversion, ARPU, LTV |

The nickname "Pirate Metrics" comes from the pronunciation: AARRR.

---

## How to use it

AARRR is a **diagnostic tool, not a prioritization tool**. Run it before RICE or BTD to answer: *which stage is broken?*

**Step 1 — Map your current metrics to each stage.** You don't need perfect data. A rough estimate per stage is enough to see the shape of the funnel.

**Step 2 — Find the biggest drop.** Where does the funnel lose the most people? That's your bottleneck.

**Step 3 — Anchor your North Star there.** If the drop is at Activation, your North Star should measure activation. If it's Retention, measure retention. Features that don't address the bottleneck stage are distractions.

**Step 4 — Prioritize within that stage.** Now run RICE — scoped to ideas that address the identified bottleneck.

---

## B2B adaptation

AARRR was built for B2C / self-serve products. In B2B, each stage maps differently:

| Stage | B2C meaning | B2B meaning |
|-------|-------------|-------------|
| Acquisition | Individual signup | Winning a company deal (sales + marketing) |
| Activation | First user action | First meaningful action by the admin or key user; successful rollout to employees |
| Retention | Individual returns | Company renews; employees use the product regularly |
| Referral | User tells friends | Champion refers other companies; case studies; expansion within a group |
| Revenue | Individual pays | Contract value, upsell, expansion |

**Implication:** In B2B, "Acquisition" is largely outside the product crew's control (it belongs to Sales and Marketing). If your bottleneck is Acquisition, the product answer is often *differentiation* (BTD) rather than a specific feature. Features that help close deals — demos, differentiators, onboarding speed — are the lever.

---

## The bottleneck principle

Fixing a stage that isn't the bottleneck produces no visible improvement. If 80% of users activate but only 20% retain after 30 days — building a better onboarding guide won't move the needle. The leak is downstream.

**The test:** *"If we doubled performance at this stage, would it materially change our growth trajectory?"* If yes, it's the bottleneck. If the answer is "not really, the problem is elsewhere" — move to the real constraint.

---

## Common traps

- **Skipping the diagnostic** — jumping straight to RICE without knowing where the funnel is broken. You end up building activation features when the real problem is retention, or retention features when the problem is acquisition.
- **Optimizing the vanity stage** — Acquisition is the most visible and easiest to measure, so teams over-invest there. Retention is harder to see and higher-leverage.
- **Treating all stages as equal** — they're not. For most products, Retention is the multiplier: a product that retains well acquires more (referral, word of mouth) and revenues more (renewals, upsell).
- **Using AARRR as a framework for everything** — it's a funnel model. Products with no clear funnel (internal tools, B2B with no self-serve) need adaptation.

---

## In practice — Alan case example

Applied to the Alan case (May 2026):
- **Acquisition**: B2B — driven by enterprise deals, largely outside the insurance experience crew's scope
- **Activation**: weak hypothesis — onboarding guide addresses this; D7/D30 data needed to confirm
- **Retention**: main lever — reimbursement journal and invoice parsing both address retention by reducing friction in the most recurring interaction (claims)
- **Referral**: not a primary lever for health insurance (regulatory constraints, low switching frequency)
- **Revenue**: premium renewal — driven by retention; not a direct product lever at crew level

**Diagnostic output:** Retention is the bottleneck for the France crew. Acquisition is outside scope. This validates a retention-first prioritization — and flags that if Alan's growth gap is actually an Acquisition problem, no feature this crew builds will solve it.

---

## Related methodologies

- [RICE](rice.md) — run after AARRR to prioritize within the identified bottleneck stage
- [BTD](btd.md) — if the bottleneck is Acquisition, the answer is often differentiation, not a feature
- [OKRs](Eagle/methodologies/okrs.md) — AARRR diagnostic should inform which stage your crew OKR targets

---

_Last updated: 2026-05-12_
