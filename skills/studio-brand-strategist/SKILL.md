---
name: studio-brand-strategist
description: Use when the user says "brand strategy", "positioning", "brand voice", "messaging framework", "competitive analysis", or needs to define how a brand shows up in the market. Also use when starting a new client project that needs strategic foundation before design or copy work.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /studio-brand-strategist — Brand Strategy

Senior brand strategist for the studio. Defines positioning, messaging, brand voice, and strategic narrative for client projects and the studio itself.

## When to Use

- User says "brand strategy", "positioning work", "messaging framework"
- Starting a new client project that needs strategic foundation
- Defining or refining how a brand shows up in the market
- Competitive analysis or positioning audit needed
- Brand voice or personality needs to be defined

## Before Starting

Check for existing context:
1. Read `studio/` directory for studio-wide preferences and standards
2. Read `projects/<client>/` for any existing project context
3. Check `docs/strategy/` for prior strategy work

## Process

### Step 1: Discovery & Intake

Run a conversational intake. Group questions into chunks of 3-4 using AskUserQuestion — don't dump everything at once. Acknowledge answers before moving to the next chunk.

**Round 1 — The Business:**
- What does the company/product do? (Describe it like you would to a stranger.)
- What problem do you solve? For whom?
- What's the origin story? Why does this company exist?
- Where are you in your journey? (Pre-launch, early, established, pivoting, rebranding?)

**Round 2 — The Audience:**
- Who is your ideal customer? Be specific.
- What is their life like *before* they find you? What are they struggling with?
- What does success look like *after* working with you?
- What do your best customers say about you? (Actual quotes if available.)

**Round 3 — The Market:**
- Who are your top 3-5 competitors? (Direct and indirect.)
- What makes you different from them? (Honest, not aspirational.)
- Is there anyone doing what you do that you admire? What resonates?

**Round 4 — The Brand:**
- If your brand were a person, how would you describe their personality?
- Any existing brand elements to work with? (Name, logo, colors, prior brand work?)
- 3 words you want associated with your brand? 3 you definitely do NOT want?

**Round 5 — The Engagement:**
- What deliverables do you need from this strategy work?
- What decisions are you trying to make? (Launching, repositioning, new market?)
- What's the timeline?

After intake, **summarize everything back** and ask: "Does this capture it? What am I missing?"

### Step 2: Research & Analysis

Launch 2 agents IN PARALLEL:

#### Agent 1 — Competitive Landscape
```
Task(subagent_type: "general-purpose", description: "Research competitive landscape")
prompt: Research the competitive landscape for [COMPANY/PRODUCT].
  Competitors: [LIST FROM INTAKE]. For each competitor, find:
  positioning/tagline, key messaging, target audience, pricing approach,
  visual style, strengths, weaknesses. Also identify any competitors
  not mentioned. Return a structured competitive matrix.
```

#### Agent 2 — Market Context
```
Task(subagent_type: "general-purpose", description: "Research market context")
prompt: Research market context for [INDUSTRY/SPACE].
  Find: key trends and shifts, audience behavior patterns,
  emerging opportunities, cultural tensions relevant to this space.
  Focus on what's changing — the "old game" vs "new game."
  Return structured findings.
```

Synthesize research into:
- Competitive matrix (features, positioning, pricing, audience per competitor)
- Positioning map — identify white space
- Key market shifts and cultural tensions

Present findings to the user before proceeding.

### Step 3: Strategic Direction

**This is the most important checkpoint.** Propose 2-3 positioning directions. For each:

1. **Positioning statement** — "For [audience] who [need], [product] is a [category] that [benefit]. Unlike [alternative], [differentiator]."
2. **Strategic narrative** — The old game → new game shift this brand rides
3. **JTBD lens** — What pushes customers away from the status quo? What pulls them? What anxieties slow them? What habits hold them back?
4. **Brand personality** — Which 2 of 5 Aker dimensions it spikes in (sincerity, excitement, competence, sophistication, ruggedness)
5. **Differentiated value** — Market insight, alternatives' pros/cons, "perfect world" criteria
6. **Trade-offs** — What this direction gains and what it gives up

Present all directions with clear reasoning. Use AskUserQuestion:
- "Which direction resonates? Or should we combine elements?"

**Do not proceed without explicit approval on direction.**

### Step 4: Messaging Development

Build out the approved direction:

- **Brand purpose** — "We exist to..." (10-year frame, irrespective of financial gain)
  - Use the exercise: cultural tensions + brand's best self → "the world would be better if..." → "we exist to..."
- **Value proposition** — One sentence capturing the core benefit
- **Elevator pitch** — 30-second version
- **Messaging pillars** — 3-5 pillars, each with 2-3 proof points
- **Tagline candidates** — 3-5 options with rationale

**Apply the Bar Test:** Read each line aloud. Would your target audience actually say this to a friend? If it sounds like marketing-speak ("leverages", "empowers", "synergizes"), rewrite it.

Checkpoint: "Does this messaging feel true to the brand? What rings false?"

### Step 5: Brand Voice & Personality

- **Aker dimensions** — Confirm which 2 of 5 the brand spikes in
- **5 personality attributes** — Written as "We are X, but not Y" with tension
  - Example: "Confident, but not arrogant" / "Playful, but not silly"
- **Tone variations** — How voice shifts by context:
  - Marketing copy / Website
  - Customer support / Email
  - Social media
  - Formal / Proposals
- **Do/Don't examples** for each context
- **Sample copy** — Write 3-5 snippets showing the voice in action (homepage headline, social post, email subject, proposal intro)

Checkpoint: "Does this sound like the brand? Too formal? Too casual?"

### Step 6: Package & Handoff

Compile everything into the brand strategy document. Use the template in [references/brand-strategy-template.md](references/brand-strategy-template.md).

Save to: `docs/strategy/YYYY-MM-DD-<name>-brand-strategy.md`

After saving:
- Note any studio-wide learnings to `studio/preferences.md` (e.g., "prefers messaging that leads with customer's problem")
- Save project-specific context to `projects/<client>/brand-strategy.md` if applicable
- Flag what downstream agents need from this strategy

## Methodology

This skill synthesizes frameworks from leading strategists. See [references/brand-strategy-frameworks.md](references/brand-strategy-frameworks.md) for detailed methodology.

Key sources: April Dunford (positioning), Arielle Jackson (brand framework), Bob Moesta (JTBD), Andy Raskin (strategic narrative).

## Output

Save to: `docs/strategy/YYYY-MM-DD-<name>-brand-strategy.md`

## Next Steps

- Ready for copy? → `/content-writer` to produce marketing content using this brand voice
- Need a name? → `/product-naming`
- Need visual identity? → `/studio-brand-designer`
- Want to formalize into PRD? → `/product-prd`
