---
name: proposal-writer
description: Write persuasive client proposals that close deals. Use when the user says "write a proposal", "client proposal", "SOW", "pitch to a client", "project proposal", "send a proposal", or needs to create a document that gets a client to say yes.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "AskUserQuestion", "Write"]
---

# /proposal-writer — The Closer

Write persuasive client proposals using narrative structure, anti-indecision tactics, and value-first framing.

## When to Use

- User says "write a proposal", "client proposal", "SOW", "project proposal"
- Need to formalize a client engagement or pitch into a document
- Converting a verbal agreement into a written proposal
- Responding to an RFP or client request

## Before Starting

Check for ALL existing project context — proposals synthesize prior work:
1. Read `projects/<project>/onboarding.md` — client context
2. Read `projects/<project>/discovery.md` — customer insight
3. Read `projects/<project>/positioning.md` — positioning and narrative
4. Read `projects/<project>/gtm-plan.md` — strategy context
5. Read `projects/<project>/pricing.md` — pricing decisions
6. Read `projects/<project>/pitch.md` — sales narrative
7. Read `projects/<project>/design-direction.md` — design context

The more prior work exists, the stronger the proposal. Flag gaps: "I don't see positioning work — the proposal will be stronger if we clarify positioning first."

## Process

### Step 1: Intake — Proposal Context

```
AskUserQuestion:
  question: "What kind of proposal is this?"
  header: "Type"
  options:
    - label: "New client engagement"
      description: "First project with this client — need to build trust and sell the approach"
    - label: "Expansion / upsell"
      description: "Existing client — proposing additional work or a new phase"
    - label: "RFP response"
      description: "Responding to a formal request for proposal"
    - label: "Internal proposal"
      description: "Pitching a project internally to leadership or stakeholders"
```

Then gather:
- **Client** — Who is this for? What do they do?
- **The opportunity** — What's the project? What problem does it solve for them?
- **Budget context** — Any sense of their budget range or expectations?
- **Decision makers** — Who will read this? Who signs off?
- **Competition** — Are they evaluating other options?
- **Urgency** — Why now? What's the timeline pressure?
- **Scope** — What would you deliver? (Phases, deliverables, timeline)
- **Pricing** — What are you planning to charge? (If undecided, we'll design pricing in the proposal)

### Step 2: Identify the MOO (Most Obvious Objection)

Before writing anything, identify the #1 reason this client might say no:

- Too expensive?
- Not sure about ROI?
- Already working with someone?
- Not the right time?
- Don't know/trust you?
- Scope too big/small?

The proposal must address this head-on, not hope they don't notice.

### Step 3: Write the Proposal

Follow the narrative structure — **sales before logistics** (Wes Kao):

#### Executive Summary (1 page)
- Open with their situation (show you understand their world)
- Name the opportunity or shift (Raskin: old game → new game)
- State your recommendation in one sentence
- Expected outcomes (specific, measurable)
- Investment range
- Timeline overview

#### Section 1: Understanding Your Situation (1-2 pages)
- Demonstrate deep understanding of their business and challenge
- Reference specific things they've said (pull from onboarding/discovery)
- Use "What Is / What Could Be" contrast (Duarte):
  - Current state: the pain, the cost, the missed opportunity
  - Future state: what's possible, what success looks like
- Name the stakes — what happens if they don't act

#### Section 2: Our Approach (2-3 pages)
- Lead with methodology, not a feature list
- Break into clear phases:
  - Phase 1: [Name] — [Duration] — [Key deliverables]
  - Phase 2: [Name] — [Duration] — [Key deliverables]
  - Phase 3: [Name] — [Duration] — [Key deliverables]
- For each phase: what we do, what you get, what we need from you
- Include milestones and decision points between phases
- **Offer your recommendation** (Dixon): "We recommend starting with Phase 1 because [reason]. This gives us [outcome] before committing to Phase 2."

#### Section 3: Expected Outcomes (1 page)
- Specific, measurable results tied to their goals
- Timeline to results
- How you'll measure success together
- **Take risk off the table** (Dixon):
  - "We'll review progress at [milestone]. If we're not on track, we'll [adjust/pause]."
  - Success metrics defined upfront so there are no surprises

#### Section 4: Investment (1 page)
- Frame as investment, not cost: "For $X, you get [specific value]"
- Include the ROI ratio: "Our clients typically see [Y]x return within [Z] months"
- Clear pricing structure:
  - What's included
  - What's not included (manages scope creep expectations)
  - Payment terms and milestones
- If appropriate, offer options (Good/Better/Best — Ramanujam)
- **Address the MOO** — If price is the objection, frame against the cost of inaction

#### Section 5: Why Us (1 page)
- Relevant experience only (not your full company history)
- 2-3 brief case studies (3 sentences each: situation → approach → result)
- The specific team members on this project and why they're right for it
- Any relevant credentials, but keep it brief

#### Section 6: Next Steps (half page)
- Specific action: "To move forward, [sign and return / schedule a call / reply with questions]"
- Timeline: "If we kick off by [date], we'll deliver [first milestone] by [date]"
- Contact info

### Step 4: Quality Check

Before presenting, verify:

- [ ] **Narrative check** — Does it tell a story about the client's future, not a catalog of your services?
- [ ] **MOO addressed** — Is the main objection handled directly?
- [ ] **Specificity check** — Are there specific numbers, dates, and examples? (Not "improve your business")
- [ ] **"So what?" test** — Does every paragraph answer "why should the client care?"
- [ ] **Skim test** — Can an executive get the gist in 2 minutes by reading bold text and headers?
- [ ] **Value before logistics** — Does the vision come before the details?
- [ ] **Risk reduction** — Are there guarantees, milestones, or off-ramps that make it safe to say yes?
- [ ] **Clean close** — Is the next step crystal clear?

### Step 5: Present and Iterate

Present the proposal draft. Checkpoint:

```
AskUserQuestion:
  question: "How does this proposal feel? What needs to change?"
  header: "Review"
  options:
    - label: "Strong — minor edits"
      description: "Narrative and positioning are right, just need polish"
    - label: "Wrong tone"
      description: "Too formal, too casual, or doesn't sound like us"
    - label: "Scope issues"
      description: "The scope, phases, or deliverables need adjustment"
    - label: "Pricing concerns"
      description: "Need to rethink the pricing or framing"
```

Iterate until the proposal is ready to send.

### Step 6: Save

Save to: `projects/<project>/proposal.md`

## Methodology

See [references/proposal-structure.md](references/proposal-structure.md) for detailed proposal methodology.

Key sources: Andy Raskin (narrative structure), Matt Dixon (reducing indecision / JOLT), Wes Kao (sales before logistics, MOO), Nancy Duarte (what is / what could be contrast).

## Output

Save to: `projects/<project>/proposal.md`

## Next Steps

- Proposal accepted? → `/onboarding` to kick off the project
- Need to refine pricing? → `/gtm-pricing`
- Need to build the pitch for an in-person presentation? → `/sales-pitch`
