---
name: gtm-strategy
description: Build comprehensive GTM plans including growth model, channel strategy, launch sequencing, team model, tech stack, metrics framework, and operating rhythm. Use when the user says "GTM plan", "go to market", "growth strategy", "launch strategy", "how do we get customers", or needs to plan how a product reaches its market.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-strategy — GTM Strategy & Plan

Build the master GTM plan: growth model, beachhead strategy, channel strategy, launch sequencing, metrics framework, team model, tech stack, and operating rhythm.

## When to Use

- User says "GTM plan", "go to market", "growth strategy", "launch strategy"
- Planning a product launch or market entry
- Evaluating growth channels and distribution
- Building a growth model (PLG vs SLG vs community vs hybrid)

## Before Starting

Check for existing context (in priority order):
1. Read `projects/<project>/gtm-context.md` — master context (REQUIRED)
2. Read `projects/<project>/icp-personas.md` — buyer profiles
3. Read `projects/<project>/competitive-intel.md` — competitive landscape
4. Read `projects/<project>/positioning.md` — positioning and messaging

If `gtm-context.md` does not exist, tell the user: "I need GTM context first. Run `/onboarding` or give me the basics."

Positioning is critical input. If it doesn't exist, flag: "A GTM plan without positioning is building on sand. Consider `/gtm-positioning` first."

## Process

### Step 1: Intake — GTM Context

Reference existing context, then gather any gaps:

```
AskUserQuestion:
  question: "What are we taking to market? And where are you in the journey?"
  header: "Stage"
  options:
    - label: "Pre-launch"
      description: "Haven't launched yet — planning initial GTM"
    - label: "Early traction"
      description: "Some users/customers but need repeatable GTM"
    - label: "Growth stage"
      description: "PMF established, need to accelerate"
    - label: "New market"
      description: "Existing product, entering a new segment or geography"
```

Gather: product, current state, business model, budget reality, sales model.

### Step 2: Research — Parallel Market Intelligence

Launch 3 agents IN PARALLEL:

#### Agent 1 — Competitive GTM Analysis
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitive GTM")
prompt: Research how competitors in [SPACE] acquire customers. Competitors: [LIST].
  For each: primary GTM motion, pricing model, key channels, sales team structure, notable tactics.
  Return a GTM competitive matrix.
```

#### Agent 2 — Channel & Distribution Research
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research distribution channels")
prompt: Research best distribution channels for [PRODUCT] targeting [AUDIENCE].
  Where does this audience discover new products? Which channels worked for similar products?
  Cost structure of each channel, earned vs paid mix, emerging channels (AEO, AI discovery).
  Return ranked channel recommendations.
```

#### Agent 3 — Growth Model Patterns
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research growth models")
prompt: Research which growth model fits [PRODUCT/BUSINESS].
  PLG, SLG, community-led, content-led, hybrid — which fits and why?
  Examples of similar companies and their models.
  Return analysis with recommendations.
```

### Step 3: Synthesize — GTM Strategy

**1. Growth Model Recommendation**
- Primary motion (PLG / SLG / community / content / hybrid)
- Why this fits (product, audience, deal size, budget)
- Prerequisites for this model to work

**2. Beachhead Strategy** (Moore)
- Target beachhead: narrowest viable segment to dominate first
- Why: compelling problem, referenceable, word-of-mouth density
- Whole product requirements
- Bowling pin plan: adjacent segments after beachhead

**3. Channel Strategy**
- Primary channels (2-3 max) with timeline, resources, key metrics
- Secondary channels to test
- Channels to avoid and why

**4. Launch Sequencing**
- Phase 1: Foundation (build before launch)
- Phase 2: Soft launch (limited audience, gather signal)
- Phase 3: Public launch (full market push)
- Phase 4: Scale (double down on what works)

**5. Fuel & Engine Audit** (Kramer)
- Fuel status: positioning, messaging, content ready?
- Engine status: distribution, operations, automation ready?
- Gaps to close before GTM can work

**6. Metrics Framework**
- North star metric
- Leading indicators (weekly: website traffic, demo requests, trial starts)
- Lagging indicators (monthly: pipeline, revenue, win rate, CAC)
- Diagnostic metrics (per channel and stage)
- Benchmarks for stage/model

**7. Team Model**
- Current team assessment
- Roles needed (with specific job descriptions, not just titles)
- Hiring sequence (which role first, second, third)
- What to outsource vs hire
- GTM engineer role if applicable (per DeWitt Grosser)

**8. Tech Stack Recommendations**
- CRM (deal tracking, pipeline management)
- Outreach/sequencing (email, LinkedIn automation)
- Analytics (attribution, funnel measurement)
- Enrichment (contact/company data)
- Content/marketing (CMS, email platform, social scheduling)

**9. Operating Rhythm**
- Weekly: pipeline review, channel metrics check
- Monthly: GTM performance review, content calendar planning
- Quarterly: strategy review, competitive intel refresh, pricing review

**10. 90-Day GTM Roadmap**
Week-by-week priorities for the first 3 months with milestones.

### Step 4: Validate with User

```
AskUserQuestion:
  question: "Does this GTM plan match your reality and resources?"
  header: "GTM Review"
  options:
    - label: "Looks right"
      description: "Matches our reality — detail it out"
    - label: "Too ambitious"
      description: "Need a leaner version for current resources"
    - label: "Wrong model"
      description: "Think a different growth model fits better"
    - label: "Missing context"
      description: "Important context to share"
```

Iterate based on feedback.

### Step 5: Save

Save to: `projects/<project>/gtm-strategy.md`

## Methodology

See [references/gtm-frameworks.md](references/gtm-frameworks.md) for detailed frameworks from DeWitt Grosser, Kramer, Moore, Verna, and Godin.

## Output

Saves to: `projects/<project>/gtm-strategy.md`

## Next Steps

- Need pricing? → `/gtm-pricing`
- Need outreach? → `/gtm-outreach`
- Need content? → `/gtm-marketing`
- Need meeting prep? → `/gtm-meeting-prep`
