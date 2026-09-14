# Metric Investigation — Internal / External Causes

> A structured framework for diagnosing unexpected metric changes in a product interview or real incident.

**Origin:** Standard PM interview framework, widely taught in interview prep (Exponent, Lenny Rachitsky, etc.). Philippe applied a version of this at Aircall.

---

## The core idea

When a metric moves unexpectedly — up or down — the instinct is to jump to a hypothesis. The framework slows that down and forces a structured search before concluding. It works in three phases: **validate the data**, **investigate internal causes**, **investigate external causes**.

The sequence matters. Diagnosing causes in a metric that isn't real wastes everyone's time.

---

## The full structure

### Phase 1 — Validate the data
Before anything else: is the change real?

- **Instrumentation issue** — did a tracking event break? Was a tag removed or renamed? Check event volume and error rates.
- **Data pipeline issue** — is there a delay, a missed batch, a transformation bug?
- **Definition change** — did the metric definition change (e.g. "active user" redefined)? Did a dashboard query change?
- **Timeframe / timezone issue** — is this a reporting artifact (day boundary, DST, week-over-week vs. month-over-month)?

*If the data isn't real, stop here and fix the instrumentation. Don't investigate causes for a ghost metric.*

---

### Phase 2 — Internal causes
Changes that originated inside the company or product.

| Category | What to check |
|----------|--------------|
| **Product changes** | Feature launches, UI changes, flow modifications shipped recently |
| **A/B tests** | New experiments running? A test with an unintended side effect? |
| **Bugs** | Regressions introduced in recent releases — especially on critical paths |
| **Infrastructure** | Latency increase, downtime, degraded performance on key flows |
| **Pricing / packaging** | Changes to plans, trial limits, paywalls |
| **Marketing / acquisition** | Campaign launch or end that changed the mix of incoming users |
| **Onboarding changes** | If activation dropped, check if onboarding flow changed |

**Key question:** *"What shipped in the last 2–4 weeks?"* Most internal causes are recent. Check the release log first.

---

### Phase 3 — External causes
Changes that originated outside the company.

| Category | What to check |
|----------|--------------|
| **Seasonality** | Same period last year — is this a recurring pattern? (holidays, fiscal year, summer) |
| **Competitor action** | New competitor launch, competitor promotion, competitor outage driving traffic your way |
| **Platform / ecosystem change** | iOS update, browser change, App Store policy, API change from a dependency |
| **Macro / market event** | Economic shock, regulatory change, news event affecting the category |
| **Partner or integration issue** | A key integration broke on the partner side |

**Key question:** *"Did anything change in the environment we don't control?"* Check industry forums, social media, competitor changelogs.

---

## Phase 4 — Segment the drop

Once you have hypotheses, segment to confirm or rule out:

- **By cohort** — new users vs. returning users? (internal acquisition change vs. retention change)
- **By geography** — one country or global? (localized cause vs. systemic)
- **By platform** — iOS vs. Android vs. web? (suggests a platform-specific bug or change)
- **By user segment** — free vs. paid, SMB vs. enterprise, specific plan?
- **By feature / flow** — which step in the funnel dropped? (narrows to a specific cause)

The segment that *didn't* drop is as informative as the one that did.

---

## Phase 5 — Hypothesize and act

By now you should have 1–3 hypotheses with supporting evidence. For each:
- State the cause clearly
- Name the supporting signal (segment, timing, correlation with release)
- Define the next action (rollback, hotfix, monitor, investigate further)

In an interview, close with: *"My top hypothesis is X because of signals Y and Z. I would validate by doing A, and if confirmed, the fix is B."*

---

## Interview application

This framework is the standard answer to: *"One of your key metrics dropped 20% overnight — walk me through how you'd investigate."*

The interviewer is testing:
1. Do you validate the data before panicking? (most candidates skip this)
2. Do you think systematically or jump to pet hypotheses?
3. Do you segment to isolate the cause?
4. Do you know how to move from diagnosis to action?

**Common mistake:** spending the whole answer on hypotheses without ever validating the data or segmenting. It signals a gut-instinct approach over a structured one.

---

## In practice — Aircall example context

At Aircall, the internal/external split was used to diagnose metric changes in a call volume or activation context. Internal causes would include changes to the dialer flow, IVR configuration, or integration with CRM tools. External causes would include telecom carrier issues, competitive moves, or macro changes in customer support staffing patterns.

---

## Related methodologies

- [North Star & Metrics](north-star-metrics.md) — knowing which metrics matter and why makes investigation faster; you know which drops are signal vs. noise
- [AARRR](aarrr.md) — AARRR tells you which funnel stage the metric lives in, which narrows the internal cause search
- [Discovery](discovery.md) — if investigation reveals a systemic user problem (not a bug), transition to discovery to understand the underlying job

---

_Last updated: 2026-05-12_
