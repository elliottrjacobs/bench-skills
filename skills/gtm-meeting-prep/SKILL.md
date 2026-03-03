---
name: gtm-meeting-prep
description: Prepare for specific sales meetings, investor meetings, or partnership conversations with account research, meeting briefs, tailored discovery questions, and follow-up templates. Use when the user says "meeting prep", "prep for a meeting", "meeting with [company]", "call prep", or has an upcoming meeting.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
argument-hint: "[company-name or meeting-type]"
---

# /gtm-meeting-prep — Meeting Preparation

Prepare for specific meetings with account research, meeting briefs, tailored discovery questions, agendas, and follow-up templates. The daily workhorse skill.

## When to Use

- User says "meeting prep", "prep for a meeting", "meeting with [company]", "call prep"
- Has an upcoming sales call, investor meeting, or partnership conversation
- Needs account-specific research before a meeting
- Wants a meeting agenda or follow-up email template

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — your product/company context
2. Read `projects/<project>/icp-personas.md` — buyer personas
3. Read `projects/<project>/competitive-intel.md` — battlecards and win/loss patterns

## Process

### Step 1: Intake — Meeting Context

If `$ARGUMENTS` provides a company or meeting type, use it. Otherwise ask:

```
AskUserQuestion:
  question: "What kind of meeting are you preparing for?"
  header: "Meeting Type"
  options:
    - label: "Sales / discovery call"
      description: "First or early conversation with a prospect"
    - label: "Demo / presentation"
      description: "Showing the product to a prospect or their team"
    - label: "Investor meeting"
      description: "Pitch to investors, board update, or fundraise"
    - label: "Partnership / strategic"
      description: "Discussion with a potential partner, integration, or strategic relationship"
```

Then gather:
- **Company name** — Who are you meeting with?
- **Person/people** — Names and titles of attendees
- **Meeting context** — How did this meeting come about? (cold outreach, inbound, referral, follow-up?)
- **Goal** — What's the ideal outcome? (Next meeting, demo, POC, decision, partnership terms?)
- **Known context** — Anything you already know about them or their situation?
- **Concerns** — Anything you're worried about or unsure how to handle?

### Step 2: Research — Parallel Account Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Company & Person Research
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research meeting target")
prompt: Research [COMPANY NAME] and [PERSON NAMES].
  Company intel:
  - What do they do? (product, market, business model)
  - Company size, funding, revenue estimates
  - Recent news (last 6 months: launches, hires, funding, partnerships)
  - Key leadership and org structure
  - Tech stack (from job postings, BuiltWith, LinkedIn)
  - Current challenges or initiatives (from earnings calls, press, blog, job postings)

  Person intel (for each attendee):
  - Current role and tenure
  - Previous companies and roles
  - Recent LinkedIn posts or activity
  - Shared connections or interests
  - What they likely care about in this meeting

  Return structured account and person brief.
```

#### Agent 2 — Industry & Competitive Context
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research industry context")
prompt: Research the industry and competitive context for a meeting with [COMPANY].
  - What industry trends affect this company?
  - What challenges do companies like this typically face?
  - Are they likely evaluating competitors? Which ones?
  - What regulatory or market forces should we be aware of?
  - What relevant case studies or proof points would resonate?
  Return structured industry context for meeting preparation.
```

### Step 3: Build — Meeting Brief

**1. Account Summary** (one paragraph)
- Who they are, what they do, why they matter

**2. Key Intelligence**
- Recent company news and what it means for the meeting
- Tech stack and current tools (relevance to your product)
- Growth signals or challenges
- Likely priorities for the people you're meeting

**3. Person Profiles** (for each attendee)
- Name, title, role in decision
- Background (previous companies, expertise)
- Likely concerns and priorities
- Rapport hooks (shared connections, interests, recent posts)

**4. Competitive Context**
- Are they likely evaluating competitors? Which ones?
- Pull relevant battlecard from competitive-intel if available
- Key differentiators to emphasize in this meeting

**5. Meeting Agenda** (with time allocations)

For a discovery call (30 min):
| Time | Section | Notes |
|------|---------|-------|
| 0-2 min | Intro & rapport | Reference rapport hooks |
| 2-5 min | Set agenda & confirm goals | "I'd love to understand X. Does that work?" |
| 5-20 min | Discovery questions | Tailored to their situation |
| 20-25 min | Initial value overview | Connect to what they shared |
| 25-30 min | Next steps | Specific ask with timeline |

For a demo (45 min):
| Time | Section | Notes |
|------|---------|-------|
| 0-3 min | Intro & agenda | |
| 3-8 min | Re-confirm pain points | "Last time we discussed X..." |
| 8-12 min | Vision / promised land | Outcomes, not features |
| 12-35 min | Live demo (their use case) | Customized to their workflow |
| 35-40 min | Proof / case study | Similar company results |
| 40-45 min | Next steps | Mutual action plan |

**6. Tailored Discovery Questions**
Customize from the discovery question bank based on:
- Their industry and company type
- Their likely challenges (from research)
- Their role in the buying committee
- What you need to learn to advance the deal

5-7 questions max — prioritized by importance.

**7. Objection Prep**
Based on their situation, anticipate 3-5 likely objections:
- The objection they'll raise
- Your response (from objection guide, customized to their context)
- Proof point relevant to them

**8. Follow-Up Email Template**

```
Subject: [Meeting recap] — [specific next step]

[Name],

Thanks for the conversation today. A few takeaways:

1. [Key pain point they shared]
2. [How you can help — tied to their specific situation]
3. [Agreed next step with specific timeline]

[Attach: relevant resource, case study, or one-pager]

Talk soon,
[Your name]
```

### Step 4: Validate

```
AskUserQuestion:
  question: "Anything I should adjust for this meeting?"
  header: "Prep Review"
  options:
    - label: "Good to go"
      description: "This covers what I need"
    - label: "Add context"
      description: "There's more I know about this account"
    - label: "Different focus"
      description: "The meeting focus is different than what's reflected"
    - label: "More questions"
      description: "Need different or additional discovery questions"
```

### Step 5: Save

Save to: `projects/<project>/meeting-prep/<company>-<YYYY-MM-DD>.md`

## Output

Saves to: `projects/<project>/meeting-prep/<company>-<date>.md`

## Next Steps

- After the meeting → Update account notes and deal status
- Need to send a proposal? → Consider bench `/proposal-writer`
- Need follow-up sequences? → `/gtm-outreach` for multi-touch follow-up
- Won the deal? → Capture as a case study with `/gtm-marketing`
