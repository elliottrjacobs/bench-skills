# AI Eval Guide

Practical guide to building evaluations for AI products. Sources: Kevin Weil (OpenAI), Hamel Husain & Shreya Shankar, Brendan Foody.

---

## What Are Evals?

Evals are systematic tests that measure how well your AI performs on specific tasks. Think of them as unit tests for AI — but instead of pass/fail, they often grade on a spectrum.

**Why they matter:**
- You can't improve what you can't measure
- Model updates (yours or the provider's) can silently degrade quality
- Evals are your north star for product quality
- "AI products are capped by how good their evals are" (Kevin Weil)

---

## Types of Evals

### 1. Accuracy Evals
Does the AI produce the correct answer?
- **Gold standard:** Human-labeled correct answers compared to model output
- **Automated:** Regex matching, exact match, fuzzy match
- **Best for:** Factual questions, classification, structured output

### 2. Quality Evals
Is the AI output good, even if there's no single "correct" answer?
- **LLM-as-Judge:** Use a strong model (e.g., Claude) to grade a weaker model's output
- **Human rating:** 1-5 scale on dimensions like helpfulness, accuracy, tone
- **Best for:** Creative writing, summaries, recommendations, conversations

### 3. Safety Evals
Does the AI avoid harmful, biased, or inappropriate outputs?
- **Refusal tests:** Does it appropriately refuse dangerous requests?
- **Bias tests:** Does it treat different groups fairly?
- **Best for:** Any user-facing AI product

### 4. Latency / Cost Evals
Is the AI fast enough and cheap enough?
- **Time to first token:** How quickly does the response start?
- **Total response time:** How long for the full output?
- **Cost per query:** What does each interaction cost?
- **Best for:** Production systems, pricing decisions

---

## Building Evals — Step by Step

### Step 1: Define Hero Use Cases
List the 10-20 most important things your AI product needs to do well. For each:
- **Input:** What the user provides (prompt, context, data)
- **Ideal output:** What a perfect response looks like
- **Failure mode:** What a bad response looks like
- **Importance:** How critical is this use case? (P0/P1/P2)

### Step 2: Create Eval Dataset
For each hero use case, create 5-20 test examples:
- Vary the inputs (different phrasings, edge cases, adversarial inputs)
- Include both easy and hard examples
- Label the expected outputs (or acceptable ranges)
- Store in a versioned dataset (CSV, JSON, or eval platform)

### Step 3: Choose Grading Method
| Eval Type | Grading Method | When to Use |
|-----------|---------------|-------------|
| Exact match | Automated string comparison | Structured output, classification |
| Fuzzy match | Semantic similarity score | Summaries, translations |
| LLM-as-Judge | Another model grades the output | Open-ended generation, creativity |
| Human review | Manual rating (1-5 scale) | High-stakes, subjective quality |
| A/B comparison | Users compare two outputs | UX optimization |

### Step 4: Establish Baselines
Run your current system against the eval dataset and record scores. This is your baseline. Every change should be measured against it.

### Step 5: Track Over Time
- Run evals on every prompt change, model update, or system change
- Watch for regressions — a model update that improves one area may degrade another
- Dashboard the results: which use cases are improving? Which are stuck?

---

## LLM-as-Judge (Detailed)

Source: Hamel Husain & Shreya Shankar, Lenny's Podcast.

### How It Works
Use a strong model to evaluate the output of your product's model:

```
System: You are an expert evaluator. Grade the following AI response on a 1-5 scale.
Criteria: [specific criteria for this use case]

User Query: {original user input}
AI Response: {model output to evaluate}
Ideal Response: {gold standard, if available}

Grade (1-5) and explain your reasoning.
```

### Best Practices
- **Be specific about criteria** — "Is this helpful?" is too vague. "Does this answer include the user's specific data point, cite a source, and provide a next step?" is grading-ready.
- **Use rubrics** — Define what 1, 3, and 5 look like for each criterion
- **Calibrate with humans first** — Have humans grade 50-100 examples, then check if LLM-as-Judge agrees
- **Use multiple criteria** — Grade on 3-5 dimensions separately, then combine (accuracy, helpfulness, safety, tone)
- **Watch for bias** — LLMs tend to be generous graders. Calibrate against human ratings.

---

## Eval-Driven Product Development

### The Workflow
1. **Define** the use case and ideal output
2. **Build** the eval dataset
3. **Measure** current performance (baseline)
4. **Iterate** on prompts/models/architecture
5. **Measure** again — did it improve?
6. **Ship** when evals hit your quality bar
7. **Monitor** in production — evals don't stop at launch

### Decision Framework
| Eval Score | Product Decision |
|------------|-----------------|
| < 60% | Don't ship — fundamental quality issue |
| 60-80% | Ship with guardrails (human review, confidence thresholds) |
| 80-95% | Ship with monitoring and fallback |
| > 95% | Ship confidently, automate |

These thresholds vary by use case. A medical AI needs 99%+. A creative writing assistant can ship at 70%.

### Key Insight
"The companies that win in AI aren't the ones with the best models — they're the ones with the best evals." Evals are your product quality flywheel: better evals → better products → better data → better models → better products.
