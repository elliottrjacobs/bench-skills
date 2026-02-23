---
name: client-discovery
description: Run customer and market discovery using JTBD methodology, opportunity mapping, and PMF measurement. Use when the user says "customer research", "discovery", "JTBD analysis", "understand the customer", "who is our customer", or needs to deeply understand a target audience before building.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /client-discovery — Customer & Market Discovery

Run structured customer and market discovery using Jobs to Be Done, Opportunity Solution Trees, and PMF measurement frameworks.

## When to Use

- User says "customer research", "discovery", "JTBD analysis", "understand the customer"
- Starting a new project and need to understand the target audience
- Evaluating product-market fit for an existing product
- Mapping the customer journey to find opportunities

## Before Starting

Check for existing project context:
1. Read `projects/` directory for any project onboarding context
2. Check `docs/strategy/` for prior strategy or discovery work
3. If a project name is mentioned, read `projects/<project>/onboarding.md` for context gathered by `/onboarding`

## Process

### Step 1: Intake — What Are We Discovering?

If project context exists from `/onboarding`, use it as the starting point. Otherwise, gather essentials:

```
AskUserQuestion:
  question: "What product or business are we researching? And what's the key question you need answered?"
  header: "Discovery"
  options:
    - label: "Who should we build for?"
      description: "Identify and prioritize target segments"
    - label: "Why do people buy/switch?"
      description: "Understand the forces driving adoption and churn"
    - label: "Where's the opportunity?"
      description: "Map the customer journey to find gaps and friction"
    - label: "Do we have PMF?"
      description: "Measure product-market fit and identify the ideal segment"
```

Then gather:
- **The product/business** — What does it do? Who is it for today?
- **The hypothesis** — What do you believe about the customer that you want to validate or challenge?
- **Existing data** — Any customer interviews, NPS scores, churn data, or survey results?
- **Competitive context** — What alternatives do customers use today?

Summarize back and confirm before proceeding.

### Step 2: Research — Parallel Market Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Market Landscape
```
Task(subagent_type: "general-purpose", description: "Research market landscape")
prompt: Research the market landscape for [PRODUCT/BUSINESS].
  - Who are the main competitors and alternatives (including "do nothing")?
  - How do competitors position themselves? What do they promise?
  - What are customers saying in reviews, forums, social media? (Look for struggling moments and unmet needs)
  - What are the key trends and shifts in this space?
  Return structured findings with sources.
```

#### Agent 2 — Customer Behavior Patterns
```
Task(subagent_type: "general-purpose", description: "Research customer behavior")
prompt: Research customer behavior patterns for [TARGET AUDIENCE] in [SPACE].
  - What jobs are these customers hiring products to do?
  - What are common struggling moments that trigger searching for solutions?
  - What alternatives do people use today? What do they love/hate about each?
  - What does the buying journey look like? (First thought → active looking → deciding → first use)
  Return structured findings organized by JTBD framework.
```

### Step 3: Synthesize — Build the Discovery Map

Using research results + intake context, build:

**1. Jobs to Be Done Analysis**
For each job identified:
- The job statement: "When [situation], I want to [motivation], so I can [outcome]"
- Push forces (what's driving change)
- Pull forces (what's attracting them to solutions)
- Anxiety forces (what's holding them back)
- Habit forces (what keeps them in the status quo)

**2. Opportunity Map**
Using Teresa Torres' Opportunity Solution Tree format:
- Business outcome we're targeting
- Customer opportunities (unmet needs / pain points / desires) — ranked by frequency and severity
- Potential solutions per opportunity (at least 2-3 per opportunity)
- Riskiest assumptions to test

**3. Customer Segments**
- Who are the "very disappointed" users (or would-be users)? — The Rahul Vohra PMF segment
- What do they have in common? (Demographics, behaviors, context)
- What language do they use to describe their problem and the value they get?

**4. Competitive Landscape**
- How customers currently solve this job (including "do nothing")
- Strengths and weaknesses of each alternative
- White space — where no current solution serves well

### Step 4: Validate — Check with the User

Present the discovery map and check:

```
AskUserQuestion:
  question: "Does this match your understanding? What surprises you or feels off?"
  header: "Validation"
  options:
    - label: "Spot on"
      description: "This matches what I've seen — let's move forward"
    - label: "Partially right"
      description: "Some things resonate but others feel off — let me clarify"
    - label: "Missing context"
      description: "I have customer data or insights that should inform this"
```

Iterate based on feedback.

### Step 5: Recommendations

Based on the discovery, provide:

1. **Target segment recommendation** — Who to focus on and why (the PMF segment)
2. **Top 3 opportunities** — Ranked by impact and feasibility
3. **Key assumptions to test** — What needs to be true, and how to test it cheaply
4. **Interview guide** — 5-7 questions to ask in customer interviews, anchored in past behavior (not opinions)
5. **Competitive positioning implications** — How the discovery should inform positioning

## Methodology

This skill synthesizes frameworks from leading practitioners. See [references/discovery-frameworks.md](references/discovery-frameworks.md) for detailed methodology.

Key sources: Teresa Torres (Opportunity Solution Trees), Bob Moesta (JTBD / 4 Forces), Rahul Vohra (PMF Engine), Gia Laudi (Customer-Led Growth).

## Output

Save discovery brief to: `projects/<project>/discovery.md`

If no project context exists, present findings inline.

## Next Steps

- Have a target segment? → `/positioning` to define how you show up
- Need pricing? → `/pricing-packaging` to design monetization
- Ready for GTM? → `/gtm-strategist` to plan your go-to-market
- Need to formalize? → `/product-prd` to write the requirements doc
