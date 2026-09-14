# The 3-Move Strategy Framework

A repeatable method for evaluating any product or business strategy question — useful in PM interviews, case studies, or real decisions.

---

## Where it comes from

This isn't a single named framework — it's a synthesis of how strong strategists actually think, drawn from several traditions:

- **Barbara Minto's Pyramid Principle** (McKinsey) — lead with a clear answer, support it with structured reasoning
- **Charlie Munger's mental models** — name the archetype first, then reason by analogy
- **Roger Martin's "Playing to Win"** — strategy is a cascade of choices, and every choice has a falsifiable condition
- **Amazon's "working backwards" / two-pizza team culture** — force a verdict, then define what would prove you wrong

Most PM interview prep teaches frameworks (CIRCLES, RICE, jobs-to-be-done) that help you organise information. This framework is different: it helps you *reason to a defensible position*.

---

## The 3 Moves

### Move 1 — Name the pattern

Before evaluating anything, identify *what type of decision this actually is*. This unlocks historical precedent immediately.

**Ask yourself:** "What archetype does this situation match?"

| Pattern | Description | Classic examples |
|---------|-------------|-----------------|
| Closed → open platform | Removing ecosystem lock-in to expand TAM | Apple → Spotify, Peloton app |
| Bundle → unbundle | Breaking a suite into standalone products (or reverse) | Microsoft Office, Notion |
| Land and expand | Enter cheap/free, monetise depth over time | Slack, Figma, Dropbox |
| Race to the bottom | Competing on price erodes margins for everyone | Airlines, cloud storage |
| Switching cost play | Stickiness through integration depth, not product quality | Salesforce, Workday |
| Platform vs. product | Build the marketplace vs. own the supply | Airbnb vs. hotel chain |
| Vertical integration | Own more of the value chain | Apple silicon, Amazon logistics |

**Why it matters:** Naming the pattern means you don't have to reason from scratch. You already know how this movie tends to end, and under what conditions it succeeds or fails.

---

### Move 2 — Steel-man both sides

Force yourself to write the strongest version of *for* and *against* — not strawmen.

**The discipline:** imagine two smart people in the room. One is a well-informed investor who backed this decision. One is a well-informed short-seller who thinks it's a mistake. What is each of their *best* arguments?

This does two things:
1. Stops you from just arguing your gut reaction
2. Shows the interviewer (or your team) that you understand the real trade-offs, not just the obvious ones

**Template:**

```
Arguments for:
• [Strongest case, quantified if possible]
• [Second-order benefit often overlooked]
• [Why the timing makes sense now]

Arguments against:
• [The fatal flaw if execution fails]
• [Who gets hurt and how badly]
• [What historical precedent suggests caution]
```

---

### Move 3 — Verdict with a condition

Avoid "it depends" as a conclusion. Instead: **"[Position], but only if [condition]."**

The condition is usually about:
- **Execution** — "this works if they manage it as a brand change, not a product change"
- **Timing** — "correct now because X, but would have been wrong 3 years ago"
- **A specific metric** — "succeeds if app subscriber LTV exceeds hardware buyer LTV within 24 months"

This is what separates a PM answer from a consultant answer. You commit to a position while showing you understand what could make you wrong.

---

## Bonus: the falsification question

After your verdict, ask: **"What's the one number I'd look at in 12 months to know if this worked?"**

This forces falsifiable thinking. If you can't name a metric, your verdict isn't really a verdict — it's a feeling.

---

## Examples

### Peloton — The Platform Pivot

**Pattern:** Closed ecosystem → open platform (Apple model → Spotify model)

**For:** Expands TAM from 4M hardware owners to 100M+ fitness app market. Removes $2,500 acquisition barrier. Software margins vastly outperform hardware.

**Against:** Hardware buyers have low churn; app subscribers at $13/month churn easily. Brand dilution — "Peloton" meant premium hardware. Real cannibalization risk on bike sales.

**Verdict:** Structurally correct (survival required broadening revenue), but requires a distinct sub-brand for the app to protect hardware premium positioning.

**Falsification metric:** App subscriber 12-month retention vs. hardware owner 12-month retention. If they converge, the thesis holds.

---

### Notion — The All-in-One Bet

**Pattern:** Bundle play / switching cost strategy

**For:** If Notion becomes the single source of truth for a team, switching cost is extreme — you'd have to replace everything simultaneously. Like Microsoft Office: individually beatable, but the bundle is durable.

**Against:** Notion will always be outgunned by specialists. As Jira/Confluence/Airtable improve their collaboration layers, "deep specialist + good-enough collab" becomes a credible competitor.

**Verdict:** Sustainable only if teams actually use multiple features together. If most users live in one feature, the bundle isn't sticky — it's just complexity.

**Falsification metric:** % of active teams using 3+ distinct Notion features. Below 40%, the switching cost thesis is weak.

---

### Spotify — Podcast Acquisition Spree (2019–2021)

**Pattern:** Vertical integration / owned content moat

**For:** Reduces dependency on music labels (who take ~70% of revenue). Owned content creates exclusive reasons to choose Spotify over Apple Music.

**Against:** Podcast production is expensive and hits are unpredictable. Listeners are not loyal to platforms — they follow shows. Exclusivity alienates audiences used to open RSS.

**Verdict:** Directionally right (need a margin escape hatch from labels), but exclusive content is the wrong vehicle. The win condition was platform infrastructure (discovery, ads), not owning shows.

**Falsification metric:** Podcast-attributed new subscriber rate. If podcasts aren't moving subscriber numbers, the content spend isn't justified.

---

## When to use this

- PM strategy interviews ("how would you evaluate X's decision to…")
- Product reviews where leadership is debating a pivot
- Any time someone says "should we do X?" and the honest answer is more nuanced than yes/no
- Reading business news — run every major company decision through these 3 moves as a habit

The goal isn't to always be right. It's to reason clearly, commit to a position, and know exactly what would change your mind.
