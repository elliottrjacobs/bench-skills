---
name: gtm-outreach
description: Build multi-channel sales outreach sequences, prospecting strategy, personalization frameworks, and reply handling playbooks. Use when the user says "outreach", "cold email", "sequences", "prospecting", "pipeline generation", "SDR playbook", or needs to build outbound sales motions.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-outreach — Sales Outreach & Pipeline Generation

Build multi-channel outreach sequences, prospecting strategy, personalization frameworks, and reply handling playbooks. Focused on the outbound motion — distinct from content marketing.

## When to Use

- User says "outreach", "cold email", "sequences", "prospecting", "SDR playbook"
- Building outbound pipeline generation
- Designing multi-channel sequences (email + LinkedIn + phone + video)
- Need prospecting criteria or reply handling playbooks

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master context
2. Read `projects/<project>/icp-personas.md` — buyer language and personas (critical for personalization)
3. Read `projects/<project>/competitive-intel.md` — competitive messaging gaps
4. Read `projects/<project>/positioning.md` — messaging pillars

ICP and positioning are critical for good outreach. If missing, flag it.

## Process

### Step 1: Intake — Outreach Context

```
AskUserQuestion:
  question: "What kind of outreach do you need?"
  header: "Outreach Type"
  options:
    - label: "Cold outbound"
      description: "Building outbound sequences from scratch for cold prospects"
    - label: "Warm follow-up"
      description: "Sequences for leads who've shown interest (inbound, event, referral)"
    - label: "Re-engagement"
      description: "Reviving cold leads or lost deals"
    - label: "Full playbook"
      description: "Complete outreach system — prospecting criteria, sequences, reply handling"
```

Then gather:
- **Target persona** — Who are you reaching? (Reference ICP if available)
- **Channels available** — Email, LinkedIn, phone, video? Which tools do you use?
- **Current state** — Any sequences running? What's working/not working?
- **Goal** — Meetings booked? Demos? Trials? Pipeline generated?
- **Proof points** — Case studies, metrics, or quotes to use in outreach?
- **Team** — Who's sending? Founder, SDR team, AE?

### Step 2: Research — Parallel Outreach Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Prospect Persona Research
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research prospect persona")
prompt: Research what resonates with [TARGET PERSONA] in outbound outreach.
  - What hooks get their attention? (common pain points, triggers, trends they care about)
  - What channels do they prefer? (email response rates vs LinkedIn vs phone)
  - What tone works? (casual, professional, data-driven, provocative?)
  - What turns them off? (common outreach mistakes for this persona)
  - What subject lines and opening lines have worked for similar products?
  Return structured persona-specific outreach recommendations.
```

#### Agent 2 — Competitive Outreach Analysis
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitive outreach")
prompt: Research how competitors in [SPACE] do outbound sales.
  - What messaging do their SDRs use? (check LinkedIn outreach, G2 reviews mentioning sales experience)
  - What claims do they lead with?
  - Where are the messaging gaps? (what they don't talk about)
  - What do prospects complain about in competitor outreach?
  Return competitive outreach intel with differentiation opportunities.
```

### Step 3: Build — Outreach System

**1. Prospecting Strategy**
- Target account criteria (firmographic, technographic, behavioral signals)
- Signal-based triggers (hiring for X role, raised funding, posted about Y problem, uses Z tool)
- Account prioritization framework (Tier 1/2/3 based on fit and signal strength)
- Data sources and enrichment tools

**2. Multi-Channel Sequence Architecture**

Build sequences for each scenario (cold, warm, re-engagement). Each sequence includes:

**Cold Outbound Sequence** (12-18 touches over 3-4 weeks):
- Day 1: Email 1 — The opener (under 80 words, specific hook)
- Day 2: LinkedIn connect + note
- Day 4: Email 2 — Value add (insight, data, resource)
- Day 7: LinkedIn engage (comment on their content)
- Day 9: Email 3 — Proof (case study, specific result)
- Day 11: Phone attempt + voicemail script
- Day 14: Email 4 — Different angle (new hook or persona pain point)
- Day 18: LinkedIn message (direct, reference previous touches)
- Day 21: Email 5 — Breakup (leave door open, final value)

For each email:
- Subject line (2-3 options for A/B testing)
- Body with personalization slots marked: `[COMPANY]`, `[PAIN_POINT]`, `[TRIGGER]`
- CTA (low commitment: 15-min call, quick question, relevant resource)

**3. Personalization Framework**
- **Tier 1 (1:1):** Fully custom — for top accounts. Research the person, reference their content, their company's specific situation.
- **Tier 2 (1:few):** Segment-personalized — same pain point, customize company name and trigger.
- **Tier 3 (1:many):** Template with variable insertion — industry, company size, role.

What to personalize and where to find it:
| Personalization Element | Source |
|------------------------|--------|
| Recent company news | LinkedIn, Crunchbase, news |
| Their content/posts | LinkedIn activity |
| Tech stack signals | BuiltWith, job postings |
| Trigger events | Funding, hiring, product launch |
| Mutual connections | LinkedIn |
| Industry-specific pain | G2 reviews, forums |

**4. Reply Handling Playbook**

| Reply Type | Response Strategy |
|-----------|-----------------|
| **Positive / interested** | Respond within 1 hour. Confirm next step with specific time. Don't over-explain. |
| **Objection** | Acknowledge, address briefly, redirect to value. Use language bank objection responses. |
| **Not now / timing** | Acknowledge, ask when to follow up, add to nurture sequence with specific date. |
| **Not the right person** | Thank them, ask for referral: "Who on your team handles [specific area]?" |
| **Negative / unsubscribe** | Remove immediately. Professional close: "Understood — thanks for letting me know." |
| **Auto-reply / OOO** | Note return date, schedule follow-up for 2 days after return. |

**5. A/B Testing Plan**
- Test 1: Subject lines (benefit vs curiosity vs question)
- Test 2: Opening lines (personalized vs pain-focused vs data-driven)
- Test 3: CTA (meeting ask vs resource offer vs question)
- Test 4: Sequence length (shorter vs longer)
- Minimum sample: 100 sends per variant before drawing conclusions

### Step 4: Validate

```
AskUserQuestion:
  question: "How do these sequences feel? Does the tone match your brand?"
  header: "Outreach Review"
  options:
    - label: "Strong"
      description: "Tone and approach are right — finalize"
    - label: "Too aggressive"
      description: "Need a softer, more consultative approach"
    - label: "Too generic"
      description: "Need more personalization or specific hooks"
    - label: "Wrong channel mix"
      description: "Need to adjust the channel balance"
```

### Step 5: Save

Save to: `projects/<project>/outreach/`
- `sequences/cold-outbound.md`
- `sequences/warm-follow-up.md`
- `sequences/re-engagement.md` (if applicable)
- `reply-playbook.md`
- `prospecting-criteria.md`

## Methodology

See [references/outreach-frameworks.md](references/outreach-frameworks.md) for sequence architecture, personalization frameworks, and founder-led sales methodology.

## Output

Saves to: `projects/<project>/outreach/`

## Next Steps

- Need competitive battlecards? → `/gtm-competitive-intel`
- Meeting booked? → `/gtm-meeting-prep` for specific meeting preparation
- Need content to support outreach? → `/gtm-marketing` for awareness and nurture content
