---
name: sales-pitch
description: Build sales pitches, cold emails, demo scripts, and pitch decks using proven frameworks. Use when the user says "sales pitch", "pitch deck", "cold email", "demo script", "sales narrative", "help me sell", or needs to craft a persuasive sales artifact.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /sales-pitch — Pitch Builder

Build sales pitches, cold emails, demo scripts, and pitch decks using frameworks from April Dunford, Matt Dixon, Andy Raskin, and Nancy Duarte.

## When to Use

- User says "sales pitch", "pitch deck", "cold email", "demo script"
- Building a sales narrative or pitch for a specific audience
- Crafting outreach for founder-led sales
- Preparing for a demo, investor meeting, or partnership conversation
- Translating positioning into sales artifacts

## Before Starting

Check for existing context:
1. Read `projects/<project>/onboarding.md` for intake context
2. Read `projects/<project>/positioning.md` — positioning directly drives the pitch
3. Read `projects/<project>/discovery.md` for customer insight
4. Read `projects/<project>/gtm-plan.md` for market context

**Positioning is critical input.** If no positioning exists, flag it: "A pitch without positioning is guesswork. Consider running `/gtm-positioning` first."

## Process

### Step 1: Intake — What Are We Building?

```
AskUserQuestion:
  question: "What kind of sales artifact do you need?"
  header: "Artifact"
  options:
    - label: "Sales pitch / deck"
      description: "Full pitch narrative for meetings and presentations"
    - label: "Cold email sequence"
      description: "Outreach emails for prospecting"
    - label: "Demo script"
      description: "Structured script for product demonstrations"
    - label: "One-pager / leave-behind"
      description: "Concise document summarizing your value proposition"
```

Then gather:
- **Who is the audience?** (Specific persona: title, company type, their priorities)
- **What's the context?** (Cold outreach, warm intro, follow-up, conference?)
- **What's the desired outcome?** (Book a meeting, close a deal, get a pilot, get a referral?)
- **Positioning context** — How do you describe what you do? Your differentiators? (Pull from positioning if available)
- **Proof points** — Any case studies, metrics, or customer quotes?
- **Known objections** — What concerns do prospects typically raise?

### Step 2: Research — Parallel Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Prospect/Market Context
```
Task(subagent_type: "general-purpose", description: "Research prospect context")
prompt: Research the prospect landscape for [PRODUCT] selling to [TARGET PERSONA].
  - What are the top priorities and challenges for [PERSONA] right now?
  - What language do they use to describe their problems? (LinkedIn, forums, job postings)
  - What buying signals indicate they're ready for a solution like this?
  - What objections are common in this sale? (based on competitor reviews, forums)
  Return findings organized by persona priorities and common objections.
```

#### Agent 2 — Competitive Sales Intelligence
```
Task(subagent_type: "general-purpose", description: "Research competitive sales")
prompt: Research how competitors in [SPACE] sell and pitch.
  - What does their sales messaging emphasize?
  - What case studies or proof points do they use?
  - Where are the gaps in their pitch? (weaknesses, missing claims)
  - What do customers say about the buying experience? (reviews, forums)
  Return competitive sales intelligence with opportunities to differentiate.
```

### Step 3: Build the Pitch

Based on artifact type selected in Step 1:

#### For Sales Pitch / Deck

Follow the **Dunford 8-step structure**, layered with **Raskin narrative** and **Dixon JOLT principles**:

**Slide 1-3: Setup (Raskin narrative frame)**
- The shift: "[Industry] is moving from [old game] to [new game]"
- The stakes: Winners vs. losers in this shift
- The perfect world: "An ideal solution would [criteria aligned to your strengths]"

**Slide 4: Introduction**
- One sentence: who you are and what you do

**Slide 5-8: Differentiated Value (Dunford structure)**
- For each differentiator (max 3):
  - The value it delivers (not the feature)
  - How you deliver it (brief — this is where you can show product)
  - Proof point (case study, data, quote)

**Slide 9: Objection Handling (Dixon JOLT)**
- Proactively address the top 2-3 silent concerns
- Take risk off the table: pilot, guarantee, success metrics
- Offer your recommendation: "Based on what you've shared, I'd recommend..."

**Slide 10: The Ask**
- Specific next step with timeline
- Make it easy to say yes

**Throughout:**
- Use Duarte's "What Is / What Could Be" rhythm
- Create affirmation loops: "Does this resonate with your experience?"
- Talk value, not features

#### For Cold Email Sequence

**Email 1: The Opener** (under 80 words)
- Subject: Specific reference to their situation (not "Quick question")
- Line 1: Something specific about them or their company
- Line 2-3: The hook — connect their situation to your insight
- Line 4: Low-commitment ask ("Would a 15-minute call make sense?")

**Email 2: The Value Add** (sent 3-4 days later)
- Share a relevant insight, article, or data point
- Brief connection to how you help
- Soft ask

**Email 3: The Proof** (sent 5-7 days later)
- Short case study: "[Similar company] achieved [specific result]"
- One-line connect to their situation
- Direct ask

**Email 4: The Breakup** (sent 7-10 days later)
- Acknowledge they're busy
- Restate the core value in one sentence
- Leave the door open

#### For Demo Script

Follow the **Kazanjy demo structure**:
1. Re-confirm the problem (2 min)
2. Show the promised land — outcomes, not features (3 min)
3. Live walkthrough of their use case (15 min)
4. Proof — similar company results (5 min)
5. Next steps — specific action and timeline (5 min)

### Step 4: Review and Refine

Present the draft. Apply quality checks:

- **The Bar Test** — Would your prospect naturally say these words to a colleague? If it sounds like marketing, rewrite.
- **The JOLT Check** — Does this reduce indecision or increase it? Fewer options is better. Offer your recommendation.
- **The "So What?" Test** — For every claim, ask "So what does this mean for the buyer?" If you can't answer, cut it.
- **Specificity Check** — Replace every vague claim with a specific proof point or metric.

```
AskUserQuestion:
  question: "How does this feel? What needs adjustment?"
  header: "Review"
  options:
    - label: "Strong — minor tweaks"
      description: "The structure and message are right, just need polish"
    - label: "Wrong tone"
      description: "The message is right but the voice/tone is off"
    - label: "Missing the mark"
      description: "Something about the angle or framing isn't right"
```

Iterate until the pitch feels authentic and compelling.

### Step 5: Save

Save to: `projects/<project>/pitch.md` (or `cold-emails.md`, `demo-script.md` depending on artifact)

## Methodology

See [references/pitch-frameworks.md](references/pitch-frameworks.md) for detailed frameworks.

Key sources: April Dunford (8-step pitch), Matt Dixon (JOLT method), Andy Raskin (strategic narrative), Nancy Duarte (storytelling structure), Pete Kazanjy (founder-led sales).

## Output

Save to: `projects/<project>/pitch.md`

## Next Steps

- Need to formalize the deal? → `/proposal-writer` to write the proposal
- Need pricing for the pitch? → `/gtm-pricing`
- Need the full brand context? → `/brand-strategist`
