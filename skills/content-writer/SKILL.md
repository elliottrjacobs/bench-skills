---
name: content-writer
description: Write marketing content — social posts, ad copy, landing pages, emails, blog posts, case studies, and more. Use when the user says "write a post", "social media copy", "ad copy", "landing page", "email campaign", "blog post", "newsletter", "case study", "marketing copy", "write content", or needs to produce customer-facing marketing materials.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /content-writer — Marketing Content Writer

Write ready-to-use marketing content using proven copywriting frameworks from Schwartz, Ogilvy, Miller, Wiebe, Handley, and Cialdini.

## When to Use

- User says "write a post", "social media copy", "ad copy", "blog post", "email campaign"
- Producing customer-facing marketing materials for any channel
- Translating brand strategy and positioning into published content
- Creating content variants for A/B testing

## Before Starting

Check for existing context:
1. Read `projects/<project>/brand-strategy.md` — voice, personality, messaging pillars, and the "For Copywriter" handoff
2. Read `projects/<project>/positioning.md` — positioning statement, differentiators, narrative
3. Read `projects/<project>/discovery.md` — customer JTBD, struggling moments, desired outcomes
4. Read `projects/<project>/gtm-plan.md` — channel strategy and audience priorities

**Brand strategy is critical input.** If no brand strategy exists, flag it: "Writing without a defined voice is guesswork. Consider running `/studio-brand-strategist` first — or tell me about the brand voice and I'll work with what we have."

If brand strategy exists, extract and apply throughout:
- Voice do's and don'ts
- Tone by context (the table maps tone to channel — use it)
- Personality attributes ("X, but not Y")
- Messaging pillars and proof points

## Process

### Step 1: Intake — What Are We Writing?

```
AskUserQuestion:
  question: "What type of content do you need?"
  header: "Content type"
  options:
    - label: "Social media posts"
      description: "LinkedIn, Twitter/X, Instagram, or other platform posts"
    - label: "Ad copy"
      description: "Google Ads, Meta Ads, display ads, or other paid media"
    - label: "Landing page / website copy"
      description: "Hero sections, feature blocks, CTAs, about pages"
    - label: "Email campaign / newsletter"
      description: "Drip sequences, announcements, newsletters, nurture emails"
```

Then gather specifics:
- **Platform/channel** — Which specific platform or placement?
- **Audience** — Who is this for? (Pull from discovery/positioning if available)
- **Goal** — What should the reader do after reading? (click, sign up, share, buy, learn)
- **Key messages** — What 1-3 things must this communicate?
- **Tone preferences** — Anything beyond brand voice? (more casual, more urgent, etc.)
- **Constraints** — Word/character count, required CTA, compliance requirements, hashtags, SEO keywords
- **Awareness level** — How familiar is the audience with the product? (See Schwartz's 5 levels in [references/copywriting-frameworks.md](references/copywriting-frameworks.md))

### Step 2: Research — Parallel Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — Audience & Channel Research
```
Task(subagent_type: "general-purpose", description: "Research audience and channel")
prompt: Research what performs well for [CONTENT TYPE] on [PLATFORM] targeting [AUDIENCE].
  - What formats and structures get engagement on this platform right now?
  - What language and tone does this audience use to describe their problems?
  - What hooks, angles, or content styles are trending?
  - What are the platform-specific best practices and constraints?
  Return findings organized by: winning formats, audience language, and platform conventions.
```

#### Agent 2 — Competitive Content Analysis
```
Task(subagent_type: "general-purpose", description: "Research competitive content")
prompt: Research how competitors in [SPACE] create [CONTENT TYPE] on [PLATFORM].
  - What topics and angles do they cover?
  - What tone and positioning do they use?
  - What gaps exist in their content? (topics they miss, angles they avoid)
  - What gets the most engagement? What falls flat?
  Return competitive content intelligence with opportunities to differentiate.
```

### Step 3: Write the Content

Select the right framework for the content type. See [references/copywriting-frameworks.md](references/copywriting-frameworks.md) for detailed frameworks.

#### Social Media Posts
- **Structure:** Hook (stop the scroll) → Value (insight, story, or proof) → CTA (engage, click, share)
- **Frameworks:** PAS for problem-focused posts, BAB for transformation stories
- **Produce 3-5 variants** with different hooks and angles
- Follow platform conventions from references (LinkedIn line breaks, Twitter thread structure, etc.)

#### Ad Copy
- **Match awareness level** (Schwartz): Most Aware gets direct offers; Unaware gets story/identity hooks
- **Headlines:** Ogilvy principles — benefit + specificity + news angle
- **Body:** Cialdini persuasion triggers — social proof, authority, scarcity (used honestly)
- **Produce 3+ headline variants and 2+ body variants** for A/B testing
- Follow character limits per platform from references

#### Landing Page / Website Copy
- **Structure:** Miller's StoryBrand — customer as hero, brand as guide
- **Hero section:** Wiebe's hierarchy — headline (benefit) + subhead (mechanism) + CTA
- **Page flow:** AIDA structure — attention → interest → desire → action
- **Include:** Social proof bar, problem agitation, 3-step plan, proof section, final CTA
- Write section by section with clear headers

#### Email Campaign / Newsletter
- **Subject lines:** 6-10 words, specific, curiosity or benefit-driven. Write 3-5 options.
- **Preview text:** Extends the subject line, doesn't repeat it
- **Body:** Handley's rules — write for one person, "so what?" every sentence, show don't tell
- **Structure:** One goal per email. One primary CTA. P.S. line for secondary hook.
- For sequences: outline the arc across emails (opener → value → proof → urgency)

#### Blog Post / Article
- **Content tilt** (Pulizzi): Find the angle no one else owns in this space
- **Structure:** Hook → context → main argument (with subheads) → takeaway → CTA
- **SEO-aware:** Include target keyword in title, H2s, and first 100 words naturally
- **Handley's rules:** Utility x Inspiration x Empathy. Start with the end in mind.

#### Case Study
- **Structure:** BAB framework — Before (the struggle) → After (the results) → Bridge (how you got there)
- **Expanded format:** Situation → Challenge → Solution → Results → Customer Quote
- **Specifics over adjectives.** "Reduced onboarding time from 3 weeks to 2 days" > "Dramatically improved onboarding"

### Step 4: Review and Refine

Apply quality checks:

- **The Bar Test** — Would the reader say this to a friend? If it sounds like marketing, rewrite.
- **The "So What?" Test** — For every claim, ask "So what does this mean for the reader?" If you can't answer, cut it.
- **Brand Voice Check** — Does this match the voice do's/don'ts and tone-by-context from brand strategy?
- **Specificity Check** — Replace every vague claim with a specific proof point, metric, or example.
- **Awareness Match** — Is this copy appropriate for how aware the audience is? (Don't pitch features to unaware readers.)

```
AskUserQuestion:
  question: "How does this feel? What needs adjustment?"
  header: "Review"
  options:
    - label: "Strong — minor tweaks"
      description: "The message and tone are right, just needs polish"
    - label: "Wrong tone"
      description: "The content is right but the voice or energy is off"
    - label: "Wrong angle"
      description: "The framing or hook isn't landing — try a different approach"
```

Iterate until the content feels authentic and ready to publish.

### Step 5: Save

Save to: `projects/<project>/content/<type>-<YYYY-MM-DD>.md` (e.g., `linkedin-posts-2025-01-15.md`, `google-ads-2025-01-15.md`)

Include at the top of the file:
- Content type and platform
- Target audience
- Key messages
- Awareness level
- Brand voice notes applied

## Methodology

See [references/copywriting-frameworks.md](references/copywriting-frameworks.md) for detailed frameworks from Schwartz, Ogilvy, Miller, Wiebe, Cialdini, Handley, and Pulizzi.

## Output

Save to: `projects/<project>/content/`

## Next Steps

- Need brand voice and messaging first? → `/studio-brand-strategist` to define the brand
- Need positioning to inform the content? → `/positioning` to define your market position
- Need to understand the customer? → `/client-discovery` for JTBD research
- Need a sales artifact instead? → `/sales-pitch` for pitches, cold emails, demo scripts
- Need to plan a content strategy? → `/gtm-strategist` for channel and launch planning
