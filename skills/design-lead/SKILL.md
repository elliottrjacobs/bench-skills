---
name: design-lead
description: Bridge strategy and design — set design direction, review design quality, and establish design tenets. Use when the user says "design review", "design direction", "design strategy", "creative direction", "design quality", "design tenets", or needs to translate strategy into design decisions.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "AskUserQuestion", "Write"]
---

# /design-lead — Design Strategy & Quality

Bridge strategy and design. Set design direction, establish design tenets, review design quality, and ensure products feel like they came from a single mind.

## When to Use

- User says "design review", "design direction", "design strategy", "creative direction"
- Translating positioning/brand strategy into design decisions
- Establishing design tenets for a new product
- Reviewing design work for quality and consistency
- Setting the creative direction before detailed design work begins

## Before Starting

Check for existing context:
1. Read `projects/<project>/onboarding.md` for project context
2. Read `projects/<project>/positioning.md` for positioning (critical input)
3. Read `projects/<project>/discovery.md` for audience insights
4. Check for existing brand guidelines or design systems
5. Check `docs/strategy/` for brand strategy work

## Process

### Step 1: Understand the Need

```
AskUserQuestion:
  question: "What kind of design leadership do you need right now?"
  header: "Need"
  options:
    - label: "Set design direction"
      description: "Establish the design vision, tenets, and creative direction for a product"
    - label: "Review design work"
      description: "Evaluate existing designs for quality, consistency, and strategy alignment"
    - label: "Bridge strategy → design"
      description: "Translate positioning and brand strategy into design decisions"
    - label: "Design system guidance"
      description: "Establish design principles, component patterns, and quality standards"
```

### Step 2: Context Gathering

Based on the need, gather relevant context:

**For setting direction:**
- What's the product? Who's it for?
- What are the brand personality attributes? (If from `/gtm-positioning`: which 2 Aaker dimensions?)
- What products do you admire? Why? (This reveals taste preferences)
- What do you want to be known for? What do you want to avoid?
- What's the competitive design landscape? (Are competitors minimal, maximalist, playful, serious?)

**For reviewing work:**
- What are the existing design tenets? (If none exist, we'll establish them first)
- What are the design files or URLs to review?
- What's the target audience and their expectations?
- What's the strategic context — what is this design trying to achieve?

**For strategy → design bridge:**
- What positioning and brand strategy exists?
- What are the key messages that need to be communicated?
- What actions should users take?
- What emotional response should the design evoke?

### Step 3: Establish Design Tenets

If no design tenets exist, create them. Tenets are **decision-making tools**, not platitudes.

**The tenet test:** Would someone reasonably argue the opposite? If not, it's a principle, not a tenet.

| Bad (platitude) | Good (tenet) |
|----------|------|
| "Simple and clean" | "Start simple — users opt into complexity" |
| "Beautiful design" | "Documentation is a failure state" |
| "User-friendly" | "Speed over polish — a fast rough feature beats a slow perfect one" |
| "Consistent" | "The product should feel like it came from a single mind" |

**Process:**
1. Identify the top 3-4 design debates that keep recurring (or will recur)
2. For each, take a clear position
3. Write each tenet as a memorable, specific statement
4. Ensure tenets create real trade-offs (gaining X means giving up Y)
5. Max 3-4 tenets — everyone must memorize them

Present tenets for approval before proceeding.

### Step 4A: Design Direction (if setting direction)

Build a design direction document covering:

**1. Design Vision**
- One paragraph describing what the product should feel like to use
- Reference the brand personality ("We are X, but not Y")
- Anchored in the user's emotional journey

**2. Design Tenets**
- 3-4 decision-making tenets (from Step 3)
- For each: the tenet, why it matters, an example of how it resolves a real decision

**3. Competitive Design Landscape**
- How competitors approach design (visual style, interaction patterns, tone)
- Where there's design white space — an unclaimed aesthetic or interaction territory

**4. First Mile Design**
- How new users should experience the product in the first 60 seconds (Belsky)
- What value they should see immediately
- Minimum decisions on the first screen

**5. Quality Bar**
- What "done" looks like — craft expectations for spacing, typography, animation, responsiveness
- Review cadence — when and how design quality is checked

### Step 4B: Design Review (if reviewing work)

Evaluate designs against these dimensions:

| Dimension | Question |
|-----------|----------|
| **Clarity** | Can users understand what to do without instruction? |
| **Consistency** | Does it feel like part of the same product? |
| **Craft** | Are details polished? Spacing, alignment, typography? |
| **Hierarchy** | Is the most important information most prominent? |
| **Efficiency** | Can users accomplish goals with minimum friction? |
| **Delight** | Is there anything that surprises or pleases? |
| **Tenet alignment** | Does it follow the product's design tenets? |
| **Strategy alignment** | Does it support the positioning and brand personality? |

**Review format:**
1. Start with what's working — name the strengths specifically
2. Identify the single biggest structural issue (not a laundry list)
3. Offer specific direction: "Try [specific change] to achieve [specific outcome]"
4. Ask before prescribing: "What were you optimizing for here?"
5. Close with priority: "If you only change one thing, make it [X]"

### Step 4C: Strategy → Design Bridge (if bridging)

Translate strategy artifacts into design decisions:

| Strategy Element | Design Decision |
|---------|--------|
| Brand personality (Aaker dimensions) | Visual tone: color warmth, typography weight, imagery style |
| "We are X, but not Y" attributes | Interaction style: playful-but-not-silly = microanimations on success states, not on every click |
| Target audience | Complexity level: developer = dense, power features; consumer = guided, progressive disclosure |
| Positioning (category + differentiator) | Information hierarchy: lead with differentiator, category sets expectations |
| Key messages | Content hierarchy: what appears above the fold, what's the headline pattern |

### Step 5: Document and Save

Save design direction or review to: `projects/<project>/design-direction.md`

Include:
- Design vision (1 paragraph)
- Design tenets (3-4)
- Key design decisions and rationale
- Quality standards and review criteria
- Reference examples (products to learn from)

## Methodology

See [references/design-principles.md](references/design-principles.md) for detailed frameworks.

Key sources: Bob Baxley (tenets, craft, Apple design culture), Katie Dill (beauty as business driver), Karri Saarinen (opinionated design, craft), Scott Belsky (first mile experience).

## Output

Save to: `projects/<project>/design-direction.md`

## Next Steps

- Need the full brand? → `/brand-strategist`
- Need a name? → `/product-naming`
- Ready to build? → `/engineer-plan` to plan implementation
- Need a proposal with design direction? → `/proposal-writer`
