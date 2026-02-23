---
name: ai-product-advisor
description: Advise on building AI-powered products — architecture, evals, UX, pricing, and go-to-market. Use when the user says "AI product", "building with AI", "AI strategy", "evals", "AI UX", "LLM product", "AI architecture", or needs guidance on incorporating AI into a product.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "Task", "AskUserQuestion", "Write"]
---

# /ai-product-advisor — AI Product Consultant

Advise on building AI-powered products: architecture decisions, eval strategy, UX patterns, pricing, and go-to-market.

## When to Use

- User says "AI product", "building with AI", "AI strategy", "evals", "AI UX"
- Evaluating whether and how to incorporate AI into a product
- Designing the AI architecture (build vs. buy, RAG vs. fine-tuning, model selection)
- Building an eval framework for an AI product
- Pricing an AI product (variable cost, outcome-based models)
- Avoiding common AI product failure modes

## Before Starting

Check for existing context:
1. Read `projects/<project>/onboarding.md` for project context
2. Read `projects/<project>/discovery.md` for customer insight
3. Read `projects/<project>/positioning.md` for market positioning
4. Check for existing technical documentation

## Process

### Step 1: Intake — AI Product Context

```
AskUserQuestion:
  question: "What's your AI product situation?"
  header: "Stage"
  options:
    - label: "Exploring AI"
      description: "Considering adding AI to an existing product or building something new with AI"
    - label: "Building an AI product"
      description: "Actively building — need architecture, eval, or UX guidance"
    - label: "Launched but struggling"
      description: "AI product is live but quality, cost, or adoption isn't where it should be"
    - label: "AI strategy"
      description: "Need a broader AI strategy for the business"
```

Then gather:
- **Product** — What does it do or what should it do?
- **The AI role** — What specific job should AI do in this product? (Generation, classification, summarization, automation, recommendations, agents?)
- **Current stack** — What models/APIs are you using? (Or considering?)
- **Data** — What proprietary data do you have? (Training data, user data, domain knowledge)
- **Users** — Who uses this and what's their tolerance for AI errors?
- **Economics** — What's the cost structure? (Inference costs, pricing model)
- **Stage** — POC, MVP, production, scaling?

### Step 2: Research — Parallel Intelligence

Launch 2 agents IN PARALLEL:

#### Agent 1 — AI Landscape & Comparable Products
```
Task(subagent_type: "general-purpose", description: "Research AI landscape")
prompt: Research the AI product landscape for [USE CASE / SPACE].
  - What comparable AI products exist? How do they work?
  - What models/approaches do they use?
  - What do users say about quality and reliability?
  - What are the pricing models (per-seat, usage, outcome)?
  - Any emerging patterns or best practices?
  Return structured competitive and landscape analysis.
```

#### Agent 2 — Technical Architecture Patterns
```
Task(subagent_type: "general-purpose", description: "Research AI architecture patterns")
prompt: Research technical architecture patterns for [AI USE CASE].
  Consider:
  - Best model choices for this use case (frontier vs. open-source, cost/quality trade-offs)
  - RAG vs. fine-tuning vs. prompt engineering — which fits best and why?
  - Agent vs. single-call architecture — when is agentic needed?
  - Common failure modes and how to mitigate them
  - Cost optimization strategies (caching, model routing, tiered inference)
  Return architecture recommendations with trade-offs.
```

### Step 3: AI Product Assessment

Evaluate the product idea against the 5 failure modes:

| Failure Mode | Risk Level | Assessment |
|-------------|-----------|------------|
| **Solution looking for a problem** | | Is there a real user need, or just cool AI? |
| **Demo-ware** | | Will it work on real data with real users? |
| **Accuracy theater** | | Are you optimizing for benchmarks or user-perceived quality? |
| **Feature, not product** | | Does AI fit into a workflow or is it a standalone trick? |
| **Cost spiral** | | Can you sustain the economics at scale? |

For each risk, provide specific mitigation recommendations.

### Step 4: Architecture Recommendation

Based on the assessment:

**1. Model Strategy**
- Recommended model(s) and why
- Build vs. buy decision (API vs. fine-tune vs. open-source)
- Model routing strategy (use cheaper models for easy tasks, frontier for hard ones)

**2. Architecture Pattern**
- Simple prompt engineering → RAG → Fine-tuning → Agent architecture
- Start with the simplest approach that works, then add complexity
- Specific recommendations for this use case

**3. Eval Strategy**
- Top 5-10 hero use cases to eval
- Recommended grading methods (LLM-as-judge, human review, automated)
- Quality thresholds for shipping (60%/80%/95% framework)
- See [references/ai-eval-guide.md](references/ai-eval-guide.md) for detailed eval methodology

**4. UX Patterns**
- How to handle AI uncertainty (confidence scores, source citations, "I'm not sure" responses)
- How to design for the failure case (what happens when AI is wrong?)
- Human-in-the-loop design (when and where to keep humans involved)
- Progressive trust building (start with suggestions → approvals → autonomous action)

**5. Pricing Implications**
- Cost per query / per action / per outcome
- Recommended pricing model (see `/pricing-packaging` for full analysis)
- Unit economics — can you sustain this at 10x current volume?

### Step 5: Roadmap Recommendations

**Phase 1: Validate** (2-4 weeks)
- Build the simplest possible version (prompt engineering + basic UI)
- Create eval dataset with 20-50 test cases
- Test with 5-10 real users on real data
- Decision: Is the AI quality good enough to proceed?

**Phase 2: Harden** (4-8 weeks)
- Expand eval dataset to 100+ cases
- Add guardrails and error handling
- Implement monitoring and logging
- Optimize cost (caching, model routing)
- Decision: Are unit economics sustainable?

**Phase 3: Scale** (8-12 weeks)
- Add RAG or fine-tuning if needed
- Build feedback loops (user corrections improve the system)
- Launch to broader audience
- Implement production evals and alerting

**Phase 4: Compound** (ongoing)
- Every user interaction should make the product better
- Build data moat through proprietary fine-tuning data
- Expand to adjacent use cases
- Move toward outcome-based pricing

### Step 6: Save

Present recommendations. Save to: `projects/<project>/ai-product-strategy.md`

## Methodology

See [references/ai-product-frameworks.md](references/ai-product-frameworks.md) for AI product methodology and [references/ai-eval-guide.md](references/ai-eval-guide.md) for eval guide.

Key sources: Kevin Weil (OpenAI product philosophy), Mike Krieger (Anthropic product), Chip Huyen (AI engineering), Aishwarya & Kiriti (AI failure modes), Elena Verna (AI growth).

## Output

Save to: `projects/<project>/ai-product-strategy.md`

## Next Steps

- Need pricing for the AI product? → `/pricing-packaging`
- Need to position the AI product? → `/positioning`
- Need to plan the GTM? → `/gtm-strategist`
- Need to scope the build? → `/engineer-plan`
