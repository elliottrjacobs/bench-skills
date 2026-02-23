---
name: pricing-packaging
description: Design pricing models, packaging tiers, and monetization strategy. Use when the user says "pricing", "packaging", "monetization", "how should we price this", "pricing model", "pricing strategy", "tiers", or needs to figure out how to charge for a product.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /pricing-packaging — Monetization Advisor

Design pricing models, packaging tiers, and monetization strategy using frameworks from Madhavan Ramanujam, Patrick Campbell, and Naomi Ionita.

## When to Use

- User says "pricing", "packaging", "monetization", "how should we price this"
- Designing pricing for a new product or feature
- Restructuring existing pricing (tiers, models, price points)
- Evaluating pricing model (seat-based vs. usage-based vs. outcome-based)
- Preparing for a price increase
- AI product pricing strategy

## Before Starting

Check for existing context:
1. Read `projects/<project>/onboarding.md` for project context
2. Read `projects/<project>/positioning.md` — positioning drives pricing
3. Read `projects/<project>/discovery.md` — customer willingness to pay
4. Read `projects/<project>/gtm-plan.md` — sales model affects pricing
5. Check `docs/strategy/` for prior pricing work

## Process

### Step 1: Intake — Pricing Context

```
AskUserQuestion:
  question: "What's the pricing situation?"
  header: "Situation"
  options:
    - label: "New product pricing"
      description: "Need to design pricing from scratch"
    - label: "Pricing restructure"
      description: "Current pricing isn't working — need to rethink"
    - label: "Price increase"
      description: "Need to raise prices without losing customers"
    - label: "AI product pricing"
      description: "Pricing an AI-powered product with variable costs"
```

Then gather:
- **Product** — What does it do? Who's it for?
- **Business model** — B2B, B2C, marketplace, API?
- **Current pricing** — What do you charge today? (If anything)
- **Cost structure** — What does it cost you to serve one customer? (Especially for AI: inference costs)
- **Competitors** — What do alternatives cost?
- **Sales model** — Self-serve, sales-assisted, enterprise?
- **Value delivered** — What tangible outcome does the customer get? Can you quantify it?

### Step 2: Research — Parallel Pricing Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Competitor Pricing Research
```
Task(subagent_type: "general-purpose", description: "Research competitor pricing")
prompt: Research pricing and packaging for competitors in [SPACE].
  Competitors: [LIST].
  For each, find:
  - Pricing model (seat, usage, flat, outcome, freemium)
  - Price points and tier structure
  - What's included in each tier (features, limits)
  - Free tier or trial structure
  - Enterprise pricing approach
  - Any recent pricing changes
  Return a structured pricing comparison matrix.
```

#### Agent 2 — Pricing Model Patterns
```
Task(subagent_type: "general-purpose", description: "Research pricing model patterns")
prompt: Research pricing model patterns for [PRODUCT TYPE] in [MARKET].
  Analyze:
  - Which pricing models are most common in this space?
  - What value metrics do successful products use? (seats, usage, events, revenue, etc.)
  - Are there examples of outcome-based pricing working here?
  - What's the typical range of price points?
  - Any emerging pricing trends (especially for AI products)?
  Return analysis with examples and recommendations.
```

### Step 3: Pricing Model Selection

Using the Ramanujam 2x2, evaluate which model fits:

**The Pricing Model 2x2:**

| | Low Attribution (hard to measure your impact) | High Attribution (easy to measure) |
|---|---|---|
| **Low Autonomy** (product assists humans) | Per-seat / flat fee | Usage-based |
| **High Autonomy** (product does the work) | Subscription tiers | **Outcome-based** ★ |

For each viable model, present:

| Model | Pros | Cons | Best When |
|-------|------|------|-----------|
| Per-seat | Simple, predictable revenue | Penalizes adoption, doesn't scale with value | Value is roughly equal per user |
| Usage-based | Scales with value, low entry barrier | Revenue unpredictable, hard to budget for buyers | Value varies widely across users |
| Outcome-based | Highest alignment, highest ceiling | Hard to measure, requires trust | High autonomy + high attribution (ideal for AI) |
| Tiered subscription | Predictable, supports segmentation | Can feel arbitrary, may under/over-charge | Clear segment differences in needs |
| Freemium | Low-friction acquisition, viral potential | Risk of free riders, hard to convert | PLG motion, network effects |

**Recommendation** — Propose 1-2 models with clear reasoning.

### Step 4: Packaging Design

Design the tier structure:

**Tier design principles:**
- 3-4 tiers max (more causes paralysis)
- Name tiers after the customer, not the features ("Starter", "Growth", "Scale", "Enterprise")
- Each tier should have a clear "this is for you" persona
- The recommended tier should be visually highlighted
- Enterprise = "Contact us" (signals premium, allows custom deals)

For each tier:
- **Name** and target persona
- **Price point** and billing cadence
- **Value metric** — what scales with value (seats, usage, etc.)
- **Included features** — what you get at this level
- **Limits** — usage caps, user limits, etc.
- **Upgrade trigger** — what makes someone naturally move to the next tier

**The expansion path** — Every tier should have a natural reason for customers to pay more over time.

### Step 5: Value Story

The price must tell a value story (Ramanujam):

- **Contextualize the price** against the value delivered
  - "For $X/month, you get Y hours back" (time savings)
  - "For $X/month, you avoid $Y in [cost]" (cost savings)
  - "For $X/month, you generate $Y in [outcome]" (revenue impact)
- **The ratio test**: Is the price less than 10% of the value delivered? If so, it's a no-brainer.
- **The Starbucks test**: Can you frame it as "less than a coffee a day"? (if consumer)
- **The ROI model**: For B2B, sketch a simple ROI showing 3-10x return

### Step 6: Validate and Present

Present the pricing recommendation:

1. **Recommended model** with rationale
2. **Tier structure** with features and price points
3. **Value story** — how to frame the price
4. **Competitive context** — how this compares
5. **Risks and mitigations** — what could go wrong
6. **Price increase path** — how to evolve pricing over time

```
AskUserQuestion:
  question: "How does this pricing feel? What concerns you?"
  header: "Pricing Review"
  options:
    - label: "Feels right"
      description: "The model and price points align with our value and market"
    - label: "Too high"
      description: "Worried about acquisition friction at these prices"
    - label: "Too low"
      description: "Worried about leaving money on the table"
    - label: "Wrong model"
      description: "Think a different pricing model would work better"
```

Iterate based on feedback. Save to: `projects/<project>/pricing.md`

## Methodology

See [references/pricing-frameworks.md](references/pricing-frameworks.md) for detailed frameworks.

Key sources: Madhavan Ramanujam (dual-engine growth, AI pricing), Patrick Campbell (value metrics, SaaS pricing), Naomi Ionita (usage vs. seat-based, monetization mistakes).

## Output

Save to: `projects/<project>/pricing.md`

## Next Steps

- Need a pitch with pricing? → `/sales-pitch`
- Need to put pricing in a proposal? → `/proposal-writer`
- Need to plan the GTM around this pricing? → `/gtm-strategist`
