---
name: onboarding
description: Gather project context through structured intake for any engagement type — GTM, Product, Design/Brand, Engineering, or General. Produces readiness scorecards and type-specific context files that all other skills consume. Use when the user says "onboarding", "new project", "new client", "set up a project", or is starting a new engagement.
allowed-tools: ["Read", "Write", "Glob", "AskUserQuestion"]
argument-hint: "[project-name]"
disable-model-invocation: true
---

# /onboarding — Smart Project Intake

Structured intake that detects engagement type, gathers relevant context, and produces readiness scorecards. Saves to type-specific context files that downstream skills consume automatically.

## When to Use

- Starting a new client engagement or personal project
- User says "onboarding", "new project", "new client", "set up a project"
- Before running any other skill for a project that hasn't been onboarded

## Process

### Step 1: Initialize Project

If `$ARGUMENTS` provides a project name, use it. Otherwise ask:

```
AskUserQuestion:
  question: "What's the project name? (Short — becomes the folder name, like 'acme' or 'solar-app')"
  header: "Project"
```

Check if `projects/<project>/onboarding.md` already exists. If it does, ask whether to update or start fresh.

### Step 2: Engagement Type

```
AskUserQuestion:
  question: "What kind of engagement is this?"
  header: "Type"
  multiSelect: true
  options:
    - label: "GTM"
      description: "Go-to-market — positioning, ICP, pricing, outreach, sales, marketing"
    - label: "Product"
      description: "Product strategy — discovery, brainstorming, PRD, tech spec"
    - label: "Design/Brand"
      description: "Brand strategy, design system, visual identity, creative direction"
    - label: "Engineering"
      description: "Technical implementation — architecture, planning, building, reviewing"
    - label: "General"
      description: "Broad engagement or not sure yet"
```

Multi-type is encouraged — most engagements span multiple areas. Store the selected types.

### Step 3: Shared Discovery — The Business

These questions apply to ALL engagement types. Ask using AskUserQuestion (group into 2-3 questions per round, acknowledge answers before continuing):

**Round 1 — What & Why:**
- What does the company/product do? (Describe it like you would to a stranger.)
- What problem do you solve? For whom?
- What stage are you at? (Idea, pre-launch, launched, scaling, pivoting, rebranding?)

**Round 2 — The Audience:**
- Who is your ideal customer? Be specific — job title, company size, situation.
- What is their life like *before* they find you? What are they struggling with?
- What does success look like *after* working with you/using your product?

**Round 3 — The Market:**
- Who are your top 3-5 competitors? (Direct and indirect — include DIY/spreadsheet/do-nothing alternatives.)
- What makes you different from them? (Honest, not aspirational.)

**Round 4 — Goals & Constraints:**
- What are you trying to accomplish with this engagement?
- What does success look like in 90 days? In 12 months?
- What constraints should we know about? (Budget, timeline, team size, technical limitations.)

### Step 4: Type-Specific Deep Dive

Based on the selected engagement types, run the relevant deep-dive rounds. If multiple types are selected, run all applicable deep dives.

#### GTM Deep Dive

**GTM Motion:**
```
AskUserQuestion:
  question: "How do you currently acquire customers?"
  header: "GTM Motion"
  options:
    - label: "Product-led (PLG)"
      description: "Self-serve signup, free tier or trial, users convert themselves"
    - label: "Sales-led (SLG)"
      description: "Sales team drives deals — SDRs, AEs, demos, proposals"
    - label: "Founder-led"
      description: "Founder is the primary salesperson, no dedicated sales team yet"
    - label: "Not sure / hybrid"
      description: "Mix of approaches or haven't figured it out yet"
```

- Sales team structure? (Founder only, SDRs + AEs, self-serve, channel partners?)
- Revenue model? (SaaS, usage-based, services, marketplace, one-time?)
- Current revenue/ARR? (Rough range: pre-revenue, <$100K, $100K-$1M, $1M-$10M, $10M+)
- Any pipeline metrics? (CAC, LTV, win rate, sales cycle length, churn rate)
- What marketing channels are active? How are they performing?

```
AskUserQuestion:
  question: "Where does your GTM feel most broken?"
  header: "Bottleneck"
  options:
    - label: "Awareness"
      description: "People don't know we exist — can't get attention"
    - label: "Pipeline"
      description: "Not enough qualified leads or demos"
    - label: "Conversion"
      description: "Leads come in but don't close — low win rate"
    - label: "Retention"
      description: "We acquire customers but they churn"
```

- How do competitors sell? (Their GTM motion, not just their product)
- Existing GTM assets? (Pitch deck, one-pagers, email sequences, case studies, sales playbook?)

#### Product Deep Dive

- What's the current product maturity? (Concept, MVP, beta, v1, established)
- Have you done user research? What did you learn?
- Any PMF signals? (Retention, NPS, word-of-mouth, organic growth)
- How do you prioritize features? (Gut, customer requests, data, framework?)
- What analytics/instrumentation do you have? What are you measuring?
- What's on the roadmap? What are the biggest product bets?
- Any technical constraints that affect product decisions?

#### Design/Brand Deep Dive

- What's your brand maturity? (No brand, basic logo/colors, partial brand system, full brand)
- Any existing visual identity? (Logo, colors, fonts, style guide?)
- If your brand were a person, how would you describe their personality?
- 3 words you want associated with your brand? 3 you do NOT want?
- Any brands outside your industry whose style/voice you admire?
- Do you have or need a design system? (None, ad hoc, partial, comprehensive)
- How do you prefer to communicate? (Formal, casual, technical, conversational?)

#### Engineering Deep Dive

- What's the tech stack? (Languages, frameworks, databases, hosting)
- Architecture overview? (Monolith, microservices, serverless, hybrid?)
- CI/CD setup? (How do you deploy? How often?)
- Testing approach? (Unit, integration, e2e? Coverage level?)
- Any performance concerns? (Load, latency, scalability?)
- Team structure? (Solo, small team, cross-functional squads?)
- What's the development workflow? (Branching strategy, code review, release process?)

### Step 5: Summarize & Confirm

After all rounds, **summarize everything back** and ask: "Does this capture it? What am I missing?"

### Step 6: Generate Readiness Scorecard(s)

For each selected engagement type, score across relevant dimensions (1-5 each). See [references/readiness-rubrics.md](references/readiness-rubrics.md) for scoring criteria.

**GTM Scorecard (8 dimensions, /40):**
1. Positioning clarity
2. ICP definition
3. Competitive understanding
4. Sales process
5. Marketing engine
6. Enablement
7. Metrics
8. Team

**Product Scorecard (6 dimensions, /30):**
1. Problem clarity
2. User research depth
3. PMF signals
4. Product analytics
5. Prioritization rigor
6. Technical foundation

**Design/Brand Scorecard (5 dimensions, /25):**
1. Brand clarity
2. Visual identity maturity
3. Design system coverage
4. Voice & tone definition
5. Brand consistency

**Engineering Scorecard (5 dimensions, /25):**
1. Architecture clarity
2. CI/CD maturity
3. Test coverage
4. Performance baseline
5. Team workflow

### Step 7: Save Context Files

**Always save** `projects/<project>/onboarding.md`:

```markdown
---
project: {name}
client: {company name}
date: {YYYY-MM-DD}
stage: {idea|pre-launch|launched|scaling|pivoting|rebranding}
engagement-types: [{gtm|product|design-brand|engineering|general}]
---

# {Project} — Onboarding

## Business Overview
{What they do, problem, stage}

## Target Audience
{Ideal customer, struggling moments, success state}

## Competitive Landscape
{Competitors, differentiation}

## Goals & Success Criteria
{Engagement goals, 90-day targets, 12-month targets}

## Constraints
{Budget, timeline, team, technical limitations}
```

**If GTM selected**, also save `projects/<project>/gtm-context.md`:

```markdown
---
project: {name}
company: {company name}
date: {YYYY-MM-DD}
stage: {pre-launch|early-traction|growth|pivot}
gtm-motion: {plg|slg|founder-led|hybrid}
---

# {Project} — GTM Context

## GTM Motion & Sales Model
{Current motion, team structure, revenue model, ARR}

## Pipeline & Metrics
{Known metrics, active channels, bottleneck}

## Market & Competition
{Competitors, how they sell, ICP hypothesis vs reality}

## Assets Inventory
{Existing GTM materials}

## GTM Readiness Scorecard
| Dimension | Score (1-5) | Notes |
|-----------|-------------|-------|
| Positioning | X | ... |
| ICP Definition | X | ... |
| Competitive Understanding | X | ... |
| Sales Process | X | ... |
| Marketing Engine | X | ... |
| Enablement | X | ... |
| Metrics | X | ... |
| Team | X | ... |
| **Overall** | **X/40** | |
```

**If Product selected**, also save `projects/<project>/product-context.md` with product-specific findings and scorecard.

**If Design/Brand selected**, also save `projects/<project>/brand-context.md` with brand-specific findings and scorecard.

**If Engineering selected**, also save `projects/<project>/engineering-context.md` with engineering-specific findings and scorecard.

### Step 8: Recommend Next Steps

Based on engagement types and scorecard gaps, recommend skills in priority order:

**GTM (prioritize lowest scorecard dimensions):**
- Positioning weak? → `/gtm-positioning`
- ICP unclear? → `/gtm-icp`
- Competitive blind spots? → `/gtm-competitive-intel`
- No GTM plan? → `/gtm-strategy`
- Pricing undefined? → `/gtm-pricing`
- No pipeline motion? → `/gtm-outreach`
- No content engine? → `/gtm-marketing`

**Product:**
- Need to understand the customer deeper? → `/client-discovery`
- Ready to brainstorm? → `/product-brainstorm`
- Need to write a PRD? → `/product-prd`
- Need a tech spec? → `/product-tech-spec`

**Design/Brand:**
- Need brand strategy? → `/brand-strategist`
- Need design direction? → `/design-lead`
- Need a design system? → `/design-system`

**Engineering:**
- Need an implementation plan? → `/engineer-plan`
- Ready to build? → `/engineer-work`
- Need a security audit? → `/security-audit`

**Sales (any type):**
- Need to pitch or propose? → `/sales-pitch` or `/proposal-writer`
- Need content? → `/content-writer`

## Output

- Always: `projects/<project>/onboarding.md`
- If GTM: `projects/<project>/gtm-context.md`
- If Product: `projects/<project>/product-context.md`
- If Design/Brand: `projects/<project>/brand-context.md`
- If Engineering: `projects/<project>/engineering-context.md`

## Next Steps

After onboarding, run the recommended skills in order. Each skill reads its relevant context file automatically.
