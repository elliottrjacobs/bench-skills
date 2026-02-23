---
name: onboarding
description: Gather project and client context that other skills reference. Use when the user says "onboarding", "new project", "new client", "set up a project", or is starting a new engagement that other skills will need context for.
allowed-tools: ["Read", "Write", "Glob", "AskUserQuestion"]
argument-hint: "[project-name]"
disable-model-invocation: true
---

# /onboarding — Project Intake

Structured intake process that gathers project and client context. Saves to a standardized file that all other skills (`/positioning`, `/gtm-strategist`, `/sales-pitch`, `/design-lead`, etc.) read before starting their work.

## When to Use

- Starting a new client engagement or personal project
- User says "onboarding", "new project", "new client", "set up a project"
- Before running any other strategy/design/sales skill for a project that hasn't been onboarded

## Process

### Step 1: Initialize Project

If `$ARGUMENTS` provides a project name, use it. Otherwise ask:

```
AskUserQuestion:
  question: "What's the project name? (This becomes the folder name — use something short like 'acme' or 'solar-app')"
  header: "Project name"
```

Check if `projects/<project>/onboarding.md` already exists. If it does, ask whether to update or start fresh.

### Step 2: Discovery — The Business

Ask using AskUserQuestion (group into chunks of 2-3, acknowledge answers before continuing):

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
- Is there anyone in your space you admire? What resonates?

**Round 4 — Goals & Constraints:**
- What are you trying to accomplish with this engagement? (Be specific: launch, reposition, grow, raise, sell.)
- What does success look like in 90 days? In 12 months?
- What constraints should we know about? (Budget, timeline, team size, technical limitations.)

**Round 5 — Existing Assets:**
- Any existing brand work? (Name, logo, colors, voice guidelines, prior strategy docs?)
- Any customer research, surveys, or testimonials available?
- Any metrics or data we should know about? (Revenue, users, NPS, churn, etc.)

After intake, **summarize everything back** and ask: "Does this capture it? What am I missing?"

### Step 3: Save Onboarding Context

Save the gathered context to `projects/$ARGUMENTS/onboarding.md` with this structure:

```markdown
---
project: {project-name}
client: {company/person name}
date: {YYYY-MM-DD}
stage: {idea|pre-launch|launched|scaling|pivoting|rebranding}
---

# {Project Name} — Onboarding

## Business Overview
{What they do, the problem they solve, origin story}

## Target Audience
{Ideal customer profile, their struggling moments, what success looks like}

## Competitive Landscape
{Competitors, differentiation, admired players}

## Goals & Success Criteria
{Engagement goals, 90-day targets, 12-month targets}

## Constraints
{Budget, timeline, team, technical limitations}

## Existing Assets
{Brand work, research, metrics, anything to build on}

## Open Questions
{Anything unclear or needing follow-up}
```

### Step 4: Recommend Next Steps

Based on the intake, suggest which skills to run next:

- Need to define positioning? → `/positioning`
- Need to understand the customer deeper? → `/client-discovery`
- Need a go-to-market plan? → `/gtm-strategist`
- Need to figure out pricing? → `/pricing-packaging`
- Need design direction? → `/design-lead`
- Building an AI product? → `/ai-product-advisor`
- Need to pitch or propose? → `/sales-pitch` or `/proposal-writer`
- Need brand strategy? → `/studio-brand-strategist`

## Output

Saves to: `projects/<project-name>/onboarding.md`

## Next Steps

After onboarding, run the relevant skills for the engagement. Each skill reads from `projects/<project>/onboarding.md` automatically.
