---
name: gtm-strategist
description: Build go-to-market plans including channel strategy, launch sequencing, and growth model. Use when the user says "go to market", "GTM plan", "launch strategy", "growth strategy", "how do we get customers", "distribution strategy", or needs to plan how a product reaches its market.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-strategist — Go-to-Market Planner

Build GTM plans including channel strategy, launch sequencing, growth model, and competitive positioning for market entry.

## When to Use

- User says "go to market", "GTM plan", "launch strategy", "growth strategy"
- Planning a product launch or market entry
- Evaluating growth channels and distribution strategy
- Building a growth model (PLG vs SLG vs community-led vs hybrid)
- Post-positioning: translating positioning into a market plan

## Before Starting

Check for existing context:
1. Read `projects/` directory for project context
2. Check `projects/<project>/onboarding.md` for intake context
3. Check `projects/<project>/positioning.md` for positioning work
4. Check `projects/<project>/discovery.md` for customer discovery
5. Check `docs/strategy/` for prior strategy work

Prior positioning and discovery work is highly valuable input — use it.

## Process

### Step 1: Intake — GTM Context

If project context exists, use it. Otherwise, gather:

```
AskUserQuestion:
  question: "What are we taking to market? And where are you in the journey?"
  header: "Stage"
  options:
    - label: "Pre-launch"
      description: "Haven't launched yet — planning initial GTM"
    - label: "Early traction"
      description: "Some users/customers but need to scale"
    - label: "Growth stage"
      description: "PMF established, need to accelerate"
    - label: "New market"
      description: "Existing product, entering a new segment or geography"
```

Then gather:
- **Product** — What does it do? Who's it for?
- **Current state** — Any existing customers? Revenue? Channels working?
- **Business model** — SaaS, marketplace, consumer, services, hybrid?
- **Budget reality** — Bootstrapped, seed, Series A+? Marketing budget?
- **Sales model** — Self-serve? Sales-assisted? Enterprise sales? Not sure?
- **Positioning** — How do you describe what you do? (If `/positioning` was done, reference it)

Summarize and confirm.

### Step 2: Research — Parallel Market Intelligence

Launch 3 agents IN PARALLEL:

#### Agent 1 — Competitive GTM Analysis
```
Task(subagent_type: "general-purpose", description: "Research competitive GTM")
prompt: Research how competitors in [SPACE] acquire customers.
  Competitors: [LIST].
  For each, find:
  - Primary GTM motion (PLG, sales-led, community, content, partnerships)
  - Pricing model (free tier, freemium, trial, demo-required, usage-based)
  - Key channels (organic, paid, partnerships, events)
  - Sales team structure (self-serve, inside sales, field sales, channel)
  - Notable GTM tactics or campaigns
  Return a structured GTM competitive matrix.
```

#### Agent 2 — Channel & Distribution Research
```
Task(subagent_type: "general-purpose", description: "Research distribution channels")
prompt: Research the best distribution channels for [PRODUCT TYPE] targeting [AUDIENCE].
  Analyze:
  - Where does this audience discover new products? (communities, publications, events, search)
  - Which channels have worked for similar products? (with examples)
  - What's the cost structure of each channel? (CAC benchmarks if available)
  - Earned vs paid channel mix for this market
  - Any emerging channels (AI-powered discovery, Answer Engine Optimization, etc.)
  Return ranked channel recommendations with reasoning.
```

#### Agent 3 — Growth Model Patterns
```
Task(subagent_type: "general-purpose", description: "Research growth models")
prompt: Research which growth model fits best for [PRODUCT/BUSINESS].
  Consider:
  - Product-Led Growth (PLG): Does the product have natural viral loops? Can users get value without talking to sales?
  - Sales-Led Growth (SLG): Is the deal size large enough? Is the buying process complex?
  - Community-Led Growth: Is there a natural community around this problem?
  - Content-Led Growth: Are buyers actively searching for solutions?
  - Hybrid models: Which combinations work in this market?
  Return analysis with examples of similar companies and their growth models.
```

### Step 3: Synthesize — GTM Strategy

Using research + positioning + discovery (if available), build:

**1. Growth Model Recommendation**
- Recommended primary motion (PLG / SLG / community / content / hybrid)
- Why this model fits (product characteristics, audience behavior, deal size, budget)
- What needs to be true for this model to work (prerequisites)

**2. Beachhead Strategy** (Moore framework)
- Target beachhead segment — narrowest viable segment to dominate first
- Why this beachhead: compelling problem, referenceable, word-of-mouth density
- Whole product requirements — what you need beyond the core product
- Bowling pin plan — which segments to knock down after the beachhead

**3. Channel Strategy**
- **Primary channels** (2-3 max) — where to focus first
- **Secondary channels** — to test and validate
- **Channels to avoid** — and why (too expensive, wrong audience, too early)
- For each primary channel: expected timeline to results, resource requirements, key metrics

**4. Launch Sequencing**
- Phase 1: Foundation (what to build before launch)
- Phase 2: Soft launch (limited audience, gather signal)
- Phase 3: Public launch (full market push)
- Phase 4: Scale (double down on what works)
- Key milestones and decision points between phases

**5. Fuel & Engine Audit** (Kramer framework)
- **Fuel status** — Is positioning/messaging/content ready?
- **Engine status** — Are distribution channels and operations ready?
- **Gaps** — What needs to be built before GTM can work?

### Step 4: Validate with User

Present the GTM strategy. Checkpoint:

```
AskUserQuestion:
  question: "Does this GTM plan feel right for your stage and resources? What concerns you?"
  header: "GTM Review"
  options:
    - label: "Looks right"
      description: "This matches our reality — let's detail it out"
    - label: "Too ambitious"
      description: "We need a leaner version for our current resources"
    - label: "Wrong model"
      description: "I think a different growth model fits us better"
    - label: "Missing context"
      description: "There's important context I should share"
```

Iterate based on feedback.

### Step 5: Detailed GTM Plan

Build out the approved direction into an actionable plan:

- **90-day GTM roadmap** — Week-by-week priorities for the first 3 months
- **Key metrics** — What to measure at each phase (leading and lagging indicators)
- **Team requirements** — Who do you need? (GTM engineer, marketer, SDR, etc.)
- **Budget allocation** — How to split resources across channels
- **Risk mitigation** — What could go wrong and contingency plans
- **Decision triggers** — When to double down, pivot channels, or change model

Save to: `projects/<project>/gtm-plan.md`

## Methodology

This skill synthesizes frameworks from leading GTM practitioners. See [references/gtm-frameworks.md](references/gtm-frameworks.md) for detailed methodology.

Key sources: Jeanne DeWitt Grosser (GTM as product), Emily Kramer (fuel vs engine), Geoffrey Moore (beachhead/chasm), Elena Verna (growth tactics), Seth Godin (minimum viable audience).

## Output

Save to: `projects/<project>/gtm-plan.md`

## Next Steps

- Need pricing? → `/pricing-packaging` to design monetization
- Need a pitch? → `/sales-pitch` to build the sales narrative
- Need content strategy? → Consider content/SEO plan as part of channel execution
- Need a proposal? → `/proposal-writer` to close the deal
