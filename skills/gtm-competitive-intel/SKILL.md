---
name: gtm-competitive-intel
description: Build comprehensive competitive intelligence dossiers including GTM analysis, battlecard drafts, and market dynamics. Use when the user says "competitive intel", "competitor analysis", "battlecards", "competitive landscape", or needs to understand how competitors sell and position.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-competitive-intel — Competitive Intelligence

Build a comprehensive competitive dossier focused on how competitors sell, not just what they sell. Produces competitive matrix, GTM analysis, battlecard drafts, and market dynamics. Supports incremental updates.

## When to Use

- User says "competitive intel", "competitor analysis", "battlecards", "competitive landscape"
- Need to understand how competitors acquire customers and position themselves
- Preparing for competitive deals or building sales enablement
- Quarterly competitive refresh

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master GTM context
2. Check if `projects/<project>/competitive-intel.md` already exists — if so, this is an UPDATE (compare to previous snapshot)
3. Check `projects/<project>/icp-personas.md` for buyer context

If `gtm-context.md` does not exist, tell the user: "I need GTM context first. Run `/onboarding` or tell me: what you sell, who you compete with, and who your buyer is."

## Process

### Step 1: Intake — Competitive Context

If GTM context exists, reference it. Then gather:

```
AskUserQuestion:
  question: "What's driving this competitive analysis?"
  header: "Context"
  options:
    - label: "Full landscape"
      description: "Need a comprehensive view of the competitive field"
    - label: "Specific competitor"
      description: "Deep-dive on 1-2 competitors we keep losing to"
    - label: "New entrant"
      description: "A new competitor has entered — need to understand them"
    - label: "Quarterly refresh"
      description: "Updating existing intel — what's changed?"
```

Then gather:
- **Competitors to analyze** — Names + URLs (include indirect alternatives: spreadsheets, agencies, doing nothing)
- **What you know already** — Any intel from sales calls, lost deals, or customer feedback?
- **Specific concerns** — Any competitor moves that worry you?

### Step 2: Research — Parallel Competitive Intelligence

Launch 3 agents IN PARALLEL:

#### Agent 1 — Product & Positioning Intelligence
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitor products")
prompt: Research competitive products in [SPACE]. Competitors: [LIST].
  For each competitor, find:
  - Product positioning and tagline
  - Key features and capabilities
  - Pricing model and price points (check pricing pages)
  - Target audience and market segment
  - Customer reviews and sentiment (G2, Capterra, TrustRadius)
  - Strengths and weaknesses from reviews
  - Recent product launches or changes
  Return a structured competitive product matrix.
```

#### Agent 2 — GTM & Sales Intelligence
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitor GTM")
prompt: Research how competitors in [SPACE] go to market. Competitors: [LIST].
  For each, find:
  - GTM motion (PLG, sales-led, community, content, hybrid)
  - Sales team structure (check LinkedIn for team size and roles)
  - Key marketing channels (content, paid, social, events, partnerships)
  - Content strategy (blog topics, frequency, gated content)
  - Outreach approach (check if SDRs are active on LinkedIn, common messaging)
  - Customer success model (self-serve, CSM, community)
  - Notable GTM tactics or recent campaigns
  Return a structured GTM competitive analysis.
```

#### Agent 3 — Market Dynamics
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research market dynamics")
prompt: Research market dynamics in [SPACE].
  Find:
  - Recent funding rounds, acquisitions, or partnerships among competitors
  - New entrants or emerging players
  - Competitors who've pivoted, struggled, or exited
  - Analyst coverage or market reports
  - Key market trends affecting the competitive landscape
  - Regulatory or technology shifts that change the playing field
  Return structured market dynamics findings.
```

### Step 3: Synthesize — Competitive Dossier

Using research + context, build:

**1. Competitive Matrix**

| | You | Competitor A | Competitor B | Competitor C | Do Nothing |
|---|---|---|---|---|---|
| Positioning | | | | | |
| Target Market | | | | | |
| Pricing Model | | | | | |
| Price Range | | | | | |
| GTM Motion | | | | | |
| Key Strengths | | | | | |
| Key Weaknesses | | | | | |
| Funding/Stage | | | | | |

**2. Competitive GTM Analysis**
For each competitor:
- How they acquire customers (channels, tactics)
- Their sales process (self-serve, demo, enterprise)
- What their messaging emphasizes
- Where they're investing (hiring, content, events)

**3. Win/Loss Patterns**
Based on reviews, forums, and user input:
- Why customers choose each competitor
- Why customers leave each competitor
- Common switching triggers
- What matters in the evaluation (from buyer perspective)

**4. Competitive Messaging Analysis**
- Claims each competitor makes
- Proof points they use
- Messaging gaps (what they DON'T say)
- Positioning vulnerabilities

**5. Battlecard Drafts** (one per major competitor)
- **Their pitch** — How they position themselves
- **Their weaknesses** — Based on reviews and analysis
- **How to win** — Your differentiators that matter against them
- **What to say** — Talk track when they come up in a deal
- **Landmines** — Questions to ask the prospect that expose competitor weaknesses
- **When you lose** — Scenarios where they're genuinely better (be honest)

**6. Market Dynamics**
- New entrants and emerging threats
- Funding and M&A activity
- Market shifts and trend implications
- Opportunities and white space

If this is an **update** (previous `competitive-intel.md` exists), add a "What Changed" section comparing to the previous snapshot.

### Step 4: Validate with User

Present the dossier. Checkpoint:

```
AskUserQuestion:
  question: "How does this match your competitive experience? What are we missing?"
  header: "Intel Review"
  options:
    - label: "Comprehensive"
      description: "This captures the landscape well — finalize it"
    - label: "Missing a competitor"
      description: "There's a competitor we should add"
    - label: "Intel is off"
      description: "Some analysis doesn't match what we see in deals"
    - label: "Need deeper dive"
      description: "Want more detail on a specific competitor"
```

Iterate based on feedback.

### Step 5: Save

Save to: `projects/<project>/competitive-intel.md`

## Methodology

See [references/competitive-frameworks.md](references/competitive-frameworks.md) for battlecard templates, competitive matrix methodology, and incremental update format.

## Output

Saves to: `projects/<project>/competitive-intel.md`

## Next Steps

- Need positioning? → `/gtm-positioning` (competitive intel directly feeds positioning)
- Need outreach? → `/gtm-outreach` (uses competitive messaging gaps for sequences)
- Ready for strategy? → `/gtm-strategy`
