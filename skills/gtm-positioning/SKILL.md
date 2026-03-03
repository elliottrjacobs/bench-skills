---
name: gtm-positioning
description: Define product positioning, messaging matrix, strategic narrative, and category strategy. Use when the user says "positioning", "messaging", "how should we position", "strategic narrative", "category design", or needs to define how a product fits in the market.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-positioning — Positioning & Messaging

Define positioning, strategic narrative, category strategy, and a messaging matrix that maps messaging by persona and buying stage. Produces a messaging guide that all downstream GTM skills consume.

## When to Use

- User says "positioning", "messaging", "how should we position", "strategic narrative"
- Defining or refining market positioning
- Building messaging for a launch, rebrand, or new market entry
- Need a messaging matrix for sales and marketing alignment

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master GTM context
2. Read `projects/<project>/icp-personas.md` — buyer personas and language bank
3. Read `projects/<project>/competitive-intel.md` — competitive landscape
4. Check if `projects/<project>/positioning.md` already exists

If `gtm-context.md` does not exist, tell the user: "I need GTM context first. Run `/onboarding` or give me the basics."

ICP and competitive intel are highly valuable. If they don't exist, flag it but proceed — you'll research during Step 2.

## Process

### Step 1: Intake — Positioning Context

Reference existing context, then gather:

```
AskUserQuestion:
  question: "What's driving this positioning work?"
  header: "Context"
  options:
    - label: "New product launch"
      description: "Defining positioning from scratch"
    - label: "Repositioning"
      description: "Current positioning isn't working — need to reframe"
    - label: "New market entry"
      description: "Taking existing product to a new audience"
    - label: "Competitive response"
      description: "A competitor shift requires repositioning"
```

Then gather:
- **What is it?** — Plain language description
- **Who is it for?** — Primary audience (reference ICP if available)
- **What alternatives exist?** — What buyers do if you don't exist
- **What makes you different?** — Honest, not aspirational
- **What's not working?** — If repositioning: current positioning and why it fails

### Step 2: Research — Parallel Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Competitive Positioning Landscape
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitive positioning")
prompt: Research how competitors position themselves in [SPACE].
  Competitors: [LIST].
  For each, find: positioning/tagline, key messaging, target audience, market category, strengths/weaknesses of their positioning.
  Also identify positioning territory no competitor owns (white space).
  Return a structured competitive positioning matrix.
```

#### Agent 2 — Market Shifts & Category Dynamics
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research market shifts")
prompt: Research market dynamics in [SPACE].
  Find: key trends changing this market (old game vs new game), how buyer expectations are shifting, emerging categories or frames, cultural tensions, white space no competitor owns.
  Return structured findings.
```

### Step 3: Synthesize — Positioning Analysis

**1. Competitive Alternatives Map**
- What buyers actually do today (including "do nothing")
- Strengths and weaknesses of each alternative
- Where each falls short (the gap)

**2. Positioning Map**
- Plot competitors on 2 key dimensions
- Identify white space — unclaimed territory
- The axes you choose define the positioning game

**3. Market Shift Analysis** (Raskin framework)
- Old Game → New Game shift
- Winners already playing the new game
- Who's stuck in the old game

### Step 4: Strategic Directions — Present 2-3 Options

For each direction, provide:

**Positioning Statement** (Dunford/Jackson)
"For [target] who [need], [product] is a [category] that [benefit]. Unlike [alternatives], [product] [differentiator]."

**Strategic Narrative** (Raskin)
- The shift: Old Game → New Game
- The stakes: winners vs. losers
- The promised land: what winning looks like
- The obstacle: why they can't get there alone
- Your role: how you help them win

**Category Strategy**
- Compete in existing category? Which one, how?
- Create a new category? Name (2-3 words max)?
- Subcategory strategy? Reframe existing category?

**Target Segment** (Moore beachhead)
- Who cares most about this positioning?
- Why best beachhead? How to expand?

**Trade-offs**
- What this direction gains and gives up
- Who it resonates with most / least

**Brand Personality** (Aaker's 5 Dimensions)
- Which 2 of 5 dimensions does this brand spike in? (Sincerity, Excitement, Competence, Sophistication, Ruggedness)
- 3-5 "We are X, but not Y" personality statements — the Y creates tension and specificity

Present all directions. Use AskUserQuestion:
```
AskUserQuestion:
  question: "Which direction resonates? Or should we combine elements?"
  header: "Direction"
```

**Do not proceed without explicit approval on direction.**

### Step 5: Build — Full Positioning Package

For the approved direction:

- **Positioning statement** — Final version
- **Strategic narrative** — Full old game → new game story
- **Value proposition** — One sentence, core benefit
- **Elevator pitch** — 30-second version
- **Messaging pillars** — 3-5 pillars with proof points each
- **Tagline candidates** — 3-5 options with rationale

**Messaging Matrix** — Map messaging by persona × buying stage:

| | Champion | Economic Buyer | Technical Evaluator |
|---|---|---|---|
| **Awareness** | [Hook/problem] | [Business impact] | [Technical trend] |
| **Consideration** | [Differentiation] | [ROI/risk] | [Architecture/integration] |
| **Decision** | [Proof/case study] | [Business case] | [Security/compliance] |

**Message Testing Recommendations**
- What to A/B test first (headlines, value props, category framing)
- How to validate positioning in market (customer conversations, landing page tests, ad creative tests)

**Bar test** — Read everything aloud. Would your target say this to a friend at a bar? If it sounds like marketing-speak, rewrite.

### Step 6: Save

Save to: `projects/<project>/positioning.md`

## Methodology

Synthesizes: April Dunford (5-component positioning), Andy Raskin (strategic narrative), Arielle Jackson (purpose/positioning/personality), Geoffrey Moore (beachhead), Christopher Lochhead (category design). See [references/positioning-frameworks.md](references/positioning-frameworks.md).

## Output

Saves to: `projects/<project>/positioning.md`

## Next Steps

- Ready for GTM plan? → `/gtm-strategy`
- Need pricing? → `/gtm-pricing`
- Need outreach? → `/gtm-outreach` (uses messaging for sequences)
- Need content? → `/gtm-marketing` (uses messaging pillars for content strategy)
