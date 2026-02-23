# AI Product Frameworks

Distilled methodology for building AI-powered products. Sources: Kevin Weil, Mike Krieger, Chip Huyen, Aishwarya & Kiriti, Elena Verna.

---

## Kevin Weil — AI Product at OpenAI

Source: Lenny's Podcast. OpenAI CPO (prev. Instagram, Twitter, Planet).

### The Fundamental Difference
"Everywhere I've ever worked before this, you kind of know what technology you're building on. The database you used this year is probably 5% better than last year. That's not true with AI. Every two months, computers can do something they've never been able to do before."

### Model Maximalism
- Don't over-invest in scaffolding around model limitations
- In 2 months, the model will be better and blow away current limitations
- "If the product you're building is right on the edge of model capabilities, keep going — you're doing something right"
- Build for where the models are going, not where they are

### Evals Are the Core PM Skill
Evals = unit tests for AI models. They're quizzes that gauge how well the model handles specific use cases.

**Why evals matter for product builders:**
- If the model gets it right 60% of the time → build a very different product than 95% or 99.5%
- You design evals alongside the product — hero use cases become eval cases
- As you fine-tune, you hill-climb on evals to improve product quality
- "AI products are capped in how amazing they can be by how good you are at evals"

**How to build good evals:**
1. Define hero use cases (the best possible outcomes for your product)
2. Create ideal inputs and ideal outputs for each
3. Grade the model's actual output against the ideal
4. Track eval scores over time — they should improve with each model/prompt iteration
5. Build evals before building the product — they define what "good" looks like

### Product Velocity at OpenAI
- Quarterly roadmapping but "I don't believe what we write down is what we'll ship 3 months from now"
- "Plans are useless. Planning is helpful" (Eisenhower)
- Bottoms-up empowered teams — don't block launches on executive review
- **Iterative deployment** — ship and learn together with the market, don't wait for perfection
- Happy making mistakes — roll back if it doesn't work

### Where Startups Can Win
- OpenAI won't go after every vertical — there are "way more smart people outside our walls"
- Industry-specific and company-specific data isn't in the training set
- "The future is incredibly smart broad-based models fine-tuned with company-specific data"
- Startups win by combining base models with domain expertise and proprietary data

---

## Mike Krieger — AI Product at Anthropic

Source: Lenny's Podcast. Anthropic CPO (prev. Instagram co-founder).

### 90% Code Written by AI
"At Anthropic, around 90% of the code in certain projects is written by Claude." This isn't a future prediction — it's the current workflow.

### MCP (Model Context Protocol)
- Open protocol for connecting AI models to external tools and data
- Think of it like USB-C for AI — a standard interface
- Enables AI agents to take actions in the real world (not just chat)
- Critical for building products where AI operates on behalf of users

### Building AI Products
- Start with the user's workflow, not the AI capabilities
- The best AI products feel like magic — the AI disappears into the experience
- Don't build "AI features" — build features that happen to use AI
- Measure success by the user's outcome, not the model's accuracy

---

## Chip Huyen — AI Engineering Fundamentals

Source: Lenny's Podcast, *AI Engineering*.

### The AI Product Stack
1. **Foundation models** — Pre-trained LLMs (GPT, Claude, Gemini, open-source)
2. **Fine-tuning** — Customizing models on your data for specific tasks
3. **RAG (Retrieval-Augmented Generation)** — Combining models with your knowledge base
4. **Prompt engineering** — Crafting inputs to get the best outputs
5. **Evals** — Measuring quality systematically
6. **Guardrails** — Safety, accuracy, and policy enforcement

### Build vs. Buy Decision
- **Use APIs** when: time-to-market matters, you don't have proprietary data advantage, capabilities are general-purpose
- **Fine-tune** when: you have proprietary data, need consistent specialized behavior, cost of API calls is too high at scale
- **Build from scratch** when: almost never (unless you're a foundation model company)

### RAG Architecture
When your AI product needs to reference specific knowledge (documents, databases, help centers):
1. Chunk your documents into digestible pieces
2. Embed them into vector representations
3. At query time, retrieve relevant chunks
4. Include retrieved context in the model's prompt
5. Generate a response grounded in your data

### Key Principles
- Start simple (prompt engineering) → add complexity only when needed (fine-tuning, RAG)
- Always have a human-in-the-loop for high-stakes decisions
- Measure everything — latency, accuracy, cost, user satisfaction
- AI engineering is still software engineering — version control, testing, monitoring all apply

---

## Why AI Products Fail — Common Patterns

Source: Aishwarya & Kiriti (50+ AI deployments), Lenny's Podcast.

### The 5 Failure Modes

**1. Solution Looking for a Problem**
- Built impressive AI capabilities without a clear user need
- "We can do X with AI!" vs. "Users need X and AI enables it"
- Fix: Start with the JTBD, not the technology

**2. Demo-ware**
- Looks amazing in demos but fails in real-world conditions
- Edge cases, messy data, user errors break it immediately
- Fix: Test with real users on real data from day one

**3. Accuracy Theater**
- Optimizing for benchmark accuracy instead of user-perceived quality
- 95% accuracy sounds great until the 5% errors destroy trust
- Fix: Build evals that match your actual use case, not generic benchmarks

**4. Feature, Not Product**
- AI capability is impressive but doesn't fit into a workflow
- Users don't change behavior for a feature — they need an integrated experience
- Fix: Map the full user workflow, not just the AI step

**5. Cost Spiral**
- Inference costs grow faster than revenue
- No pricing model that scales sustainably
- Fix: Design pricing around value delivered, monitor unit economics from day one

### What Successful AI Products Do Differently
- Start with a specific, narrow use case (not a platform)
- Build trust through transparency (show confidence scores, sources, reasoning)
- Design for the failure case (what happens when the AI is wrong?)
- Invest in evals as heavily as features
- Price based on outcomes, not usage

---

## Elena Verna — Growth for AI Companies

Source: Lenny's Podcast. Reforge, Miro, SurveyMonkey.

### AI Growth Is Different
- PMF must be "recaptured" every 3 months as models improve
- Product capabilities shift faster than user habits form
- Traditional growth playbooks (viral loops, network effects) need adaptation

### Key Principles for AI Growth
- Focus on **workflow integration** — AI features embedded in existing workflows stick better than standalone AI tools
- **Habit formation is harder** when capabilities change constantly
- Build around **jobs to be done**, not AI capabilities — the job stays stable even as the tech evolves
- **Compounding data advantage** — every user interaction should make the product better (flywheel)
