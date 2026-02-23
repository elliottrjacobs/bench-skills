---
name: positioning
description: Define product or company positioning, competitive frame, and strategic narrative. Use when the user says "positioning", "competitive positioning", "how should we position", "category design", "strategic narrative", or needs to define how a product fits in the market.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /positioning — Positioning Strategist

Define product positioning, competitive frame, and strategic narrative using frameworks from April Dunford, Andy Raskin, Arielle Jackson, and Geoffrey Moore.

## When to Use

- User says "positioning", "competitive positioning", "how should we position this"
- Defining or refining how a product/company fits in the market
- Building a strategic narrative (old game → new game)
- Deciding whether to create a new category or compete in an existing one
- Preparing positioning for a launch, rebrand, or new market entry

## Before Starting

Check for existing context:
1. Read `projects/` directory for any project context
2. Check for discovery work: `projects/<project>/discovery.md`
3. Check for brand strategy: `docs/strategy/`
4. If a project name is mentioned, read `projects/<project>/onboarding.md`

## Process

### Step 1: Intake — Positioning Context

If project context exists, use it. Otherwise, gather:

```
AskUserQuestion:
  question: "What are we positioning? And what's driving this work?"
  header: "Context"
  options:
    - label: "New product launch"
      description: "Defining positioning from scratch for something new"
    - label: "Repositioning"
      description: "Current positioning isn't working — need to reframe"
    - label: "New market entry"
      description: "Taking an existing product to a new audience or market"
    - label: "Competitive response"
      description: "A competitor shift requires us to reposition"
```

Then gather:
- **What is it?** Describe what you do in plain language.
- **Who is it for?** Who's the primary audience today?
- **What alternatives exist?** What would customers do if you didn't exist?
- **What makes you different?** Honest differentiation — not aspirational.
- **What's not working?** If repositioning: what's the current positioning and why does it fail?

Summarize back and confirm.

### Step 2: Research — Parallel Competitive Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Competitive Positioning Landscape
```
Task(subagent_type: "general-purpose", description: "Research competitive positioning")
prompt: Research how competitors position themselves in [SPACE].
  Competitors to research: [LIST].
  For each competitor, find:
  - Their positioning/tagline
  - Key messaging and value propositions
  - Target audience
  - Market category they claim
  - Strengths and weaknesses of their positioning
  Also identify any competitors not mentioned. Return a structured competitive positioning matrix.
```

#### Agent 2 — Market Shifts & Category Dynamics
```
Task(subagent_type: "general-purpose", description: "Research market shifts")
prompt: Research market dynamics and shifts in [SPACE].
  Find:
  - Key trends changing this market (the "old game" vs "new game")
  - How the buyer's world is changing
  - Emerging categories or frames
  - Cultural tensions relevant to this space
  - White space — positioning territory no competitor owns
  Return structured findings.
```

### Step 3: Synthesize — Positioning Analysis

Using research + intake, build:

**1. Competitive Alternatives Map**
- What customers actually do today (including "do nothing")
- Strengths and weaknesses of each alternative
- Where each alternative falls short (the gap)

**2. Positioning Map**
- Plot competitors on 2 key dimensions relevant to this market
- Identify white space — unclaimed territory
- Note: the axes you choose define the positioning game

**3. Market Shift Analysis**
- What's the "old game → new game" shift? (Raskin framework)
- Who are the winners already playing the new game?
- Who's stuck in the old game?

### Step 4: Strategic Directions — Present 2-3 Options

For each positioning direction, provide:

**1. Positioning Statement** (Dunford/Jackson format)
"For [target audience] who [need], [product] is a [category] that [key benefit]. Unlike [alternatives], [product] [differentiator]."

**2. Strategic Narrative** (Raskin format)
- The shift: [Old Game] → [New Game]
- The stakes: winners vs. losers
- The promised land: what winning looks like
- The obstacle: why they can't get there alone
- Your role: how you help them win

**3. Category Decision**
- Compete in existing category? Which one and how?
- Create a new category? What's the name? (2-3 words max)
- Subcategory strategy? Reframe an existing category?

**4. Target Segment** (Moore beachhead)
- Who cares most about this positioning?
- Why are they the best beachhead?
- How do we expand from there?

**5. Brand Personality** (Aaker dimensions)
- Which 2 of 5 dimensions does this brand spike in?
- 3-5 "We are X, but not Y" personality statements

**6. Trade-offs**
- What this direction gains
- What it gives up
- Who it resonates with most / least

Present all directions with clear reasoning. Use AskUserQuestion:
```
AskUserQuestion:
  question: "Which direction resonates? Or should we combine elements?"
  header: "Direction"
```

**Do not proceed without explicit approval on direction.**

### Step 5: Build Out — Full Positioning Package

For the approved direction:

- **Positioning statement** — Final version
- **Strategic narrative** — Full old game → new game story
- **Value proposition** — One sentence capturing the core benefit
- **Elevator pitch** — 30-second version
- **Messaging pillars** — 3-5 pillars, each with proof points
- **Tagline candidates** — 3-5 options with rationale
- **Bar test check** — Read everything aloud. Would your target say this to a friend at a bar? If it sounds like marketing-speak, rewrite it.

### Step 6: Validate and Save

Present the full positioning package. Checkpoint:
```
AskUserQuestion:
  question: "Does this positioning feel true? What rings false or needs adjustment?"
  header: "Validation"
```

Save to: `projects/<project>/positioning.md`

## Methodology

This skill synthesizes frameworks from leading strategists. See [references/positioning-frameworks.md](references/positioning-frameworks.md) for detailed methodology.

Key sources: April Dunford (5-component positioning), Andy Raskin (strategic narrative), Arielle Jackson (purpose/positioning/personality), Geoffrey Moore (beachhead strategy), Christopher Lochhead (category design).

## Output

Save to: `projects/<project>/positioning.md`

## Next Steps

- Need to name the product? → `/product-naming`
- Ready for GTM? → `/gtm-strategist` to plan go-to-market
- Need the full brand? → `/studio-brand-strategist` for holistic brand strategy
- Need a pitch? → `/sales-pitch` to build the sales narrative
