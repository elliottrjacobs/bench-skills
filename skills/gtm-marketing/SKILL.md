---
name: gtm-marketing
description: Create marketing content organized by funnel stage with content strategy, distribution plans, and SEO/AEO strategy. Use when the user says "content strategy", "marketing content", "blog post", "case study", "social posts", "content calendar", "SEO strategy", or needs to create content that drives pipeline.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /gtm-marketing — Marketing Content & Strategy

Create marketing content organized by funnel stage (awareness → consideration → decision → retention). Includes content strategy, distribution plans, SEO/AEO optimization, and case study frameworks.

## When to Use

- User says "content strategy", "marketing content", "blog", "case study", "social posts"
- Building a content engine for pipeline generation
- Need content for a specific funnel stage or campaign
- Planning content calendar or SEO/AEO strategy

## Before Starting

Check for existing context:
1. Read `projects/<project>/gtm-context.md` — master context
2. Read `projects/<project>/positioning.md` — messaging pillars drive content
3. Read `projects/<project>/icp-personas.md` — audience and language bank
4. Read `projects/<project>/competitive-intel.md` — content gaps to exploit
5. Read `projects/<project>/gtm-strategy.md` — channel strategy

Positioning and ICP are critical. Content without positioning is noise.

## Process

### Step 1: Intake — Content Context

```
AskUserQuestion:
  question: "What content do you need?"
  header: "Content Type"
  options:
    - label: "Content strategy"
      description: "Full content strategy — what to create, why, where, and when"
    - label: "Specific content piece"
      description: "Write a specific piece (blog, email, social, case study)"
    - label: "Case study"
      description: "Customer story or case study"
    - label: "Content calendar"
      description: "Plan what to publish over the next month/quarter"
```

Then gather:
- **Funnel stage focus** — Awareness, consideration, decision, or retention?
- **Platform/channel** — Blog, LinkedIn, email, Twitter/X, YouTube, podcast?
- **Audience** — Who is this for? (Reference ICP)
- **Goal** — Traffic, leads, demos, nurture, retention?
- **Awareness level** — Does the audience know they have this problem? (Schwartz levels)
- **Existing content** — What's already published? What's working?
- **Voice/tone** — Formal, conversational, provocative, educational?

### Step 2: Research — Parallel Content Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Audience & Channel Research
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research content audience")
prompt: Research content consumption patterns for [TARGET AUDIENCE].
  - Where do they consume content? (platforms, publications, communities)
  - What formats work? (long-form, short-form, video, podcast, newsletter)
  - What topics are they searching for? (keyword themes, questions asked)
  - What content do they share and engage with?
  - What publications or creators do they follow?
  Return audience content preferences with recommendations.
```

#### Agent 2 — Competitive Content Audit
```
Task(subagent_type: "general-purpose", model: "sonnet", description: "Research competitive content")
prompt: Research content strategy of competitors in [SPACE]. Competitors: [LIST].
  - What do they publish? (topics, frequency, formats)
  - What topics do they rank for in search?
  - What content gets the most engagement?
  - What topics do they NOT cover? (content gaps)
  - What content tilt (unique angle) could we own?
  Return competitive content analysis with gap opportunities.
```

### Step 3: Build — Content by Funnel Stage

Organize all content by funnel stage, matching Schwartz awareness levels:

**Awareness (Unaware → Problem-Aware)**
- Thought leadership on the market shift (Raskin old game → new game)
- Educational content on the problem space (not your product)
- SEO/AEO content targeting problem-aware searches
- Social posts that provoke conversation about the shift
- Format: blog, LinkedIn, Twitter threads, podcast guest appearances

**Consideration (Problem-Aware → Solution-Aware)**
- Comparison guides ("X vs Y" — honest, not biased)
- "How to choose" guides with evaluation frameworks
- Case studies with specific metrics (Before → After → Bridge)
- Webinar/demo content showing the approach (not just the product)
- Email nurture sequences moving prospects down funnel
- Format: long-form blog, email, webinar, video demos

**Decision (Solution-Aware → Most Aware)**
- Customer stories with ROI metrics
- Product-specific content (features, integrations, security)
- ROI calculators or assessment tools
- Testimonial pages organized by persona or use case
- Technical content for evaluators (API docs, architecture)
- Format: case studies, one-pagers, demo videos, technical docs

**Retention (Post-Purchase)**
- Onboarding guides and best practices
- Feature announcement content
- Community content and user spotlights
- Advanced use case content for expansion
- Newsletter for customers (different from prospect newsletter)
- Format: email, in-app, community posts, webinars

For each piece of content, include:
- **Purpose** — Why this piece exists, what metric it serves
- **Audience** — Specific persona and awareness level
- **Key message** — One sentence thesis
- **Distribution plan** — Where and how to get it seen
- **CTA** — What the reader should do next
- **SEO/AEO notes** — Target keywords, search intent, AI-optimization

### Step 4: Content Strategy Layer

If the user asked for strategy (not just a specific piece):

- **Content tilt** (Pulizzi) — The unique angle no one else owns
- **One channel, one format first** — Where to start and why
- **Content pillars** — 3-5 topic categories mapped to messaging pillars
- **Distribution plan** — 20% creation, 80% distribution
- **SEO/AEO strategy** — Target topics, not just keywords; optimize for AI discovery
- **Case study pipeline** — Framework for systematically collecting customer stories
- **Content calendar** — Monthly or quarterly publishing schedule with topics, owners, and dates

### Step 5: Quality Checks

Before finalizing any content:
- **Bar test** — Would the target audience naturally share this?
- **"So what?" test** — Every claim answers why the reader should care
- **Specificity check** — Replace vague claims with specific metrics/proofs
- **Voice check** — Does this sound like a person or a marketing department?
- **One-CTA check** — Is there exactly one primary call to action?

### Step 6: Validate and Save

```
AskUserQuestion:
  question: "How does this content feel? Does it match your brand voice?"
  header: "Content Review"
  options:
    - label: "Strong"
      description: "Tone and approach are right"
    - label: "Wrong voice"
      description: "Tone doesn't match our brand"
    - label: "Missing context"
      description: "Need to add specific proof points or examples"
    - label: "Wrong focus"
      description: "Content is targeting the wrong audience or stage"
```

Save to: `projects/<project>/content/<stage>/<type>-<YYYY-MM-DD>.md`
- `content/awareness/`
- `content/consideration/`
- `content/decision/`
- `content/retention/`

Strategy docs save to: `projects/<project>/content/content-strategy.md`

## Methodology

See [references/content-frameworks.md](references/content-frameworks.md) for copywriting frameworks (Schwartz, Ogilvy, Miller, Handley, Pulizzi, Cialdini).

## Output

Saves to: `projects/<project>/content/`

## Next Steps

- Need to write a specific content piece? → `/content-writer` for copywriting execution
- Need outreach content? → `/gtm-outreach` (distinct from marketing content)
- Need competitive battlecards? → `/gtm-competitive-intel`
