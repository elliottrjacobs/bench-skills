---
name: gtm-pricing
description: Design pricing models, tier structure, deal desk guidance, expansion revenue strategy, and pricing communication plans. Use when the user says "pricing", "how should we price", "monetization", "packaging", "tiers", "pricing page", or needs to figure out how to charge for a product.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-pricing — Pricing & Monetization

Design pricing models, tier structures, deal desk guidance, expansion revenue strategy, and pricing communication plans.

## When to Use

- User says "pricing", "monetization", "packaging", "tiers", "how should we price"
- Designing pricing from scratch or restructuring existing pricing
- Need deal desk guidance for enterprise sales
- Planning a price increase or expansion revenue strategy

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master context
2. Read `projects/<project>/positioning.md` — positioning drives pricing
3. Read `projects/<project>/icp-personas.md` — willingness to pay varies by persona
4. Read `projects/<project>/competitive-intel.md` — competitive pricing data
5. Read `projects/<project>/gtm-strategy.md` — sales model affects pricing

If `gtm-context.md` does not exist, ask for basics. Positioning is critical — flag if missing.

## Process

### Step 1: Intake — Pricing Context

```
AskUserQuestion:
  question: "What's driving this pricing work?"
  header: "Situation"
  options:
    - label: "New product"
      description: "Setting pricing for the first time"
    - label: "Restructure"
      description: "Current pricing isn't working — too complex, wrong model, or wrong price"
    - label: "Price increase"
      description: "Need to raise prices — planning the how"
    - label: "AI product"
      description: "Pricing an AI product with variable cost structure"
```

Then gather:
- Product/service, business model, current pricing (if any)
- Cost structure (fixed costs, variable costs, margins)
- Typical deal size and buyer (SMB, mid-market, enterprise)
- Value delivered (time savings, cost savings, revenue impact — be specific)
- Competitors' pricing (reference competitive intel if available)
- Current challenges with pricing

### Step 2: Research — Parallel Pricing Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Competitor Pricing Analysis
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitor pricing")
prompt: Research pricing for competitors in [SPACE]. Competitors: [LIST].
  For each: pricing model, price points, tier structure, free tier/trial, enterprise pricing approach.
  Also research pricing page design patterns. Return structured comparison.
```

#### Agent 2 — Pricing Model Patterns
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research pricing models")
prompt: Research pricing models for [PRODUCT TYPE] selling to [AUDIENCE].
  Which models work best (per-seat, usage, outcome-based, tiered, freemium)?
  Examples of successful pricing in this category. Common mistakes.
  Return analysis with recommendations.
```

### Step 3: Synthesize — Pricing Strategy

**1. Pricing Model Recommendation**

Apply the Ramanujam 2x2:
- Attribution (can you measure value per unit?) × Autonomy (does AI/product work independently?)
- Low/Low → Per-seat or flat fee
- Low/High → Subscription tiers
- High/Low → Usage-based
- High/High → Outcome-based (ideal for AI)

Recommend model with rationale.

**2. Tier Structure** (3-4 tiers max)
For each tier:
- **Name** — Named after the customer persona, not features
- **Target persona** — Who this tier is for
- **Price** — With annual/monthly breakdown
- **Value metric** — What scales with usage
- **Key features** — What's included (and what's not)
- **Limits** — Usage caps or seat limits
- **Upgrade trigger** — What naturally pushes them to the next tier

**3. Value Story**
Contextualize the price against value:
- Time savings: "X hours saved per week = $Y in productivity"
- Cost savings: "Replaces $Z in tools/headcount"
- Revenue impact: "Customers see X% increase in [metric]"
- **Ratio test:** Price should be < 10% of value delivered

**4. Pricing Page Strategy**
- Page structure and anchoring strategy
- Which tier to highlight as recommended
- Annual vs. monthly presentation
- Enterprise "Contact Us" approach
- Social proof placement
- FAQ for common pricing objections

**5. Deal Desk Guidance**
- Discounting guardrails (max discount, who can approve)
- Negotiation playbook (gives and gets per Ramanujam)
- Packaging for procurement (multi-year, prepayment, volume)
- When to hold firm vs. flex on price
- Custom enterprise pricing framework

**6. Expansion Revenue Design**
- Upgrade triggers by tier (what signals readiness for next tier)
- Upsell paths (additional seats, features, usage)
- Cross-sell strategy (complementary products/services)
- NRR target and how to achieve it

**7. Pricing Communication Strategy**
- How to announce new pricing or changes
- How to handle objections to price
- Price increase playbook (timing, messaging, grandfathering)
- Sales talk track for pricing conversations

### Step 4: Validate

```
AskUserQuestion:
  question: "Does this pricing approach feel right?"
  header: "Pricing Review"
  options:
    - label: "Looks right"
      description: "Model and structure make sense — finalize"
    - label: "Too expensive"
      description: "Worried about pricing ourselves out of the market"
    - label: "Too cheap"
      description: "Think we're leaving money on the table"
    - label: "Wrong model"
      description: "The pricing model doesn't fit our product/market"
```

### Step 5: Save

Save to: `projects/<project>/pricing.md`

## Methodology

See [references/pricing-frameworks.md](references/pricing-frameworks.md) for Ramanujam, Campbell, and Ionita frameworks.

## Output

Saves to: `projects/<project>/pricing.md`

## Next Steps

- Need outreach with pricing? → `/gtm-outreach`
- Need a pitch? → Build pricing into sales narrative
- Need a landing page? → Use pricing page strategy for landing page design
