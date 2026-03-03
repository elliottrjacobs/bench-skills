---
name: gtm-icp
description: Build deep ICP and buyer persona profiles including buying committees, triggers, process maps, negative personas, and language banks. Use when the user says "ICP", "buyer persona", "who should we sell to", "ideal customer", "buying committee", or needs actionable buyer profiles.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-icp — ICP & Buyer Persona Builder

Build actionable buyer profiles that sales, marketing, and product can all use. Goes beyond demographics to map buying committees, triggers, decision processes, and the exact language buyers use.

## When to Use

- User says "ICP", "buyer persona", "ideal customer", "who should we sell to"
- Defining or refining who to target
- Building buyer personas for a new market or segment
- Need language bank for outreach and content

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master GTM context (REQUIRED)
2. Check if `projects/<project>/icp-personas.md` already exists (update or start fresh?)

If `gtm-context.md` does not exist, tell the user: "I need GTM context first. Run `/onboarding` or give me the basics: what you sell, who you think your buyer is, and who your competitors are."

## Process

### Step 1: Intake — ICP Context

If GTM context exists, reference it. Then gather:

```
AskUserQuestion:
  question: "What's driving this ICP work?"
  header: "Context"
  options:
    - label: "Starting from scratch"
      description: "No defined ICP yet — need to build one"
    - label: "Refining existing"
      description: "Have a general sense but need to go deeper"
    - label: "New segment"
      description: "Existing product, exploring a new buyer segment"
    - label: "ICP mismatch"
      description: "Current ICP isn't working — wrong buyers, low close rates"
```

Then gather:
- **Current hypothesis** — Who do you think your ideal buyer is? (Title, company type, situation)
- **Who actually buys today?** — If you have customers, who are the best ones? What do they have in common?
- **Who should NOT buy?** — Anyone who bought and churned, or who drains support?
- **Deal size and buying process** — Typical deal size? How many people are involved in the decision?
- **Known competitors** — Who else are these buyers evaluating?

### Step 2: Research — Parallel Buyer Intelligence

Launch 3 agents IN PARALLEL:

#### Agent 1 — Review Mining & Language Bank
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Mine buyer language")
prompt: Research buyer language and sentiment for [PRODUCT TYPE] targeting [AUDIENCE].
  Mine these sources:
  - G2, Capterra, TrustRadius reviews of [COMPETITORS]
  - Reddit threads about [PROBLEM SPACE]
  - LinkedIn posts and comments from [TARGET TITLES]
  - Forum discussions about [PROBLEM]
  For each source, extract:
  - Exact phrases buyers use to describe their problems
  - Language used to describe desired outcomes
  - Complaints about existing solutions
  - Decision criteria mentioned in reviews
  Return a structured language bank organized by: problems, desired outcomes, objections, and decision criteria.
```

#### Agent 2 — Buying Behavior Research
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research buying behavior")
prompt: Research how [TARGET AUDIENCE] evaluates and purchases [PRODUCT TYPE].
  Find:
  - Typical buying committee composition (who's involved, their role in the decision)
  - Buying triggers (what events cause them to look for a solution)
  - Evaluation criteria and process (how they compare options)
  - Typical buying timeline (from first thought to purchase)
  - Common objections and deal-killers
  - Budget authority and procurement process for this buyer
  Return structured findings on the buying process.
```

#### Agent 3 — Firmographic & Technographic Patterns
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research ICP patterns")
prompt: Research the firmographic and technographic profile of ideal buyers for [PRODUCT TYPE].
  Analyze:
  - Company size patterns (employee count, revenue range) for best-fit customers
  - Industry verticals where this problem is most acute
  - Tech stack signals (what tools they use that indicate fit)
  - Organizational structure patterns (centralized vs distributed teams)
  - Growth stage indicators (hiring patterns, funding stage)
  - Geographic or regulatory factors that affect buying
  Return a structured ICP profile with specific, measurable criteria.
```

### Step 3: Synthesize — Build ICP & Personas

Using research + intake, build:

**1. ICP Definition**
- Firmographic criteria (company size, industry, revenue, stage)
- Technographic criteria (tech stack signals, tools in use)
- Behavioral criteria (growth rate, hiring patterns, recent events)
- Qualifying questions to identify fit

**2. Buying Committee Personas** (one per role)
For each persona in the buying committee:
- **Role** — Champion, Economic Buyer, Technical Evaluator, End User, Blocker
- **Title examples** — Common job titles
- **What they care about** — Their priorities and success metrics
- **How they evaluate** — What they look for, what impresses them
- **How to win them** — What messaging and proof resonates
- **How they can block** — What concerns they raise

**3. Buying Triggers**
Events that cause the buyer to enter the market — hiring spikes, funding rounds, compliance deadlines, competitive pressure, tool consolidation, new leadership, etc.

**4. Buying Process Map**
Timeline from first thought to purchase:
- Trigger → Passive research → Active evaluation → Shortlist → POC/Trial → Decision → Procurement → Go-live
- Who's involved at each stage
- What content/touchpoints matter at each stage

**5. Negative Personas**
Who NOT to sell to and why — company types, titles, or situations that consistently lead to churn, long sales cycles, or support burden.

**6. Language Bank**
Organized by category:
- **Problem language** — How buyers describe their pain
- **Outcome language** — How they describe success
- **Objection language** — Common concerns in their words
- **Decision criteria** — What they say matters when evaluating

### Step 4: Validate with User

Present the ICP and personas. Checkpoint:

```
AskUserQuestion:
  question: "Do these personas match your experience? What feels off?"
  header: "ICP Review"
  options:
    - label: "Looks right"
      description: "This matches who we sell to — let's finalize"
    - label: "Missing a persona"
      description: "There's a key player in the buying process we missed"
    - label: "Wrong priorities"
      description: "The priorities or concerns aren't quite right for our buyers"
    - label: "Too broad / too narrow"
      description: "The ICP criteria need adjustment"
```

Iterate based on feedback.

### Step 5: Save

Save to: `projects/<project>/icp-personas.md`

## Methodology

See [references/icp-frameworks.md](references/icp-frameworks.md) for buying committee mapping, negative persona framework, and language bank methodology.

## Output

Saves to: `projects/<project>/icp-personas.md`

## Next Steps

- Need competitive intelligence? → `/gtm-competitive-intel`
- Ready for positioning? → `/gtm-positioning` (ICP directly feeds positioning)
- Need outreach? → `/gtm-outreach` (uses ICP and language bank for sequences)
- Need content? → `/gtm-marketing` (uses ICP for funnel-stage content)
