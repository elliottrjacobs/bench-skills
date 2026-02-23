---
name: landing-page
description: "Create high-converting landing pages or review existing ones. Use when the user says 'landing page', 'write a landing page', 'create a landing page', 'review my landing page', 'landing page copy', 'improve my landing page', or needs to produce or critique a landing page."
allowed-tools: [WebSearch, WebFetch, Read, Write, Edit, Bash, AskUserQuestion, Task]
argument-hint: [url-to-review]
---

# /landing-page — Landing Page Creator & Reviewer

Create high-converting landing pages from scratch or review existing ones against proven best practices.

## When to Use

- User wants to write landing page copy for a product, feature, or campaign
- User wants to review or improve an existing landing page
- User says "landing page", "write a landing page", "landing page copy", "review my landing page"

## Process

### Step 1: Determine Mode

If `` contains a URL, enter **Review Mode**.
If the user explicitly asks to review/improve a page, enter **Review Mode**.
Otherwise, enter **Create Mode**.

---

## Create Mode

### Step 2: Gather Context

Ask the user the following. Skip any they've already answered in conversation:

1. **What's the product/service?** — One sentence.
2. **Who is the target audience?** — Be specific (role, pain, situation).
3. **What's the single goal of this page?** — Sign up, book a demo, buy, download, etc.
4. **What's the visitor's awareness level?** — Are they cold (don't know you), warm (know the problem), or hot (comparing solutions)?
5. **Do you have existing positioning or messaging?** — Brand voice, taglines, value props already defined.
6. **Any must-include elements?** — Specific testimonials, stats, integrations, compliance badges, etc.

### Step 3: Define Strategy

Before writing a single word, establish:

- **Value proposition** — One clear sentence: what you do, for whom, and why it matters
- **Primary CTA** — The single action you want visitors to take
- **Message match** — What promise brought them here (ad copy, email, search query)?
- **Key objections** — The 3-4 reasons someone would NOT convert. The page must address each.
- **Proof points** — Testimonials, metrics, logos, case studies available to use

Present this strategy summary to the user for approval before writing copy.

### Step 4: Write the Landing Page

Follow the proven high-converting structure. See [references/page-anatomy.md](references/page-anatomy.md) for the full breakdown.

Write each section in order:

**1. Hero Section**
- **Headline** — Benefit-driven, specific, under 10 words. This carries everything — 8/10 people read only this.
- **Subheadline** — Expands on "how" the benefit is delivered. 1-2 sentences.
- **CTA** — Action-oriented button text (not "Submit" — use "Start free trial", "Get the guide", etc.)
- **Visual direction** — Describe what image, screenshot, or demo should appear here.

**2. Social Proof Bar**
- Logos, user counts, or one standout metric ("Trusted by 10,000+ teams")

**3. Pain Section**
- Acknowledge 2-3 specific frustrations the visitor feels. Use their language.
- Use the PAS or PRESTO framework. See [references/copywriting-frameworks.md](references/copywriting-frameworks.md).

**4. Benefits (Not Features)**
- 3-4 blocks, each with a short heading and 1-2 sentences
- Format: outcome the customer gets, not what the product does
- "Ship 2x faster" not "CI/CD pipeline integration"

**5. How It Works**
- 3 simple steps. Reduce perceived complexity.
- Each step: number + short heading + one sentence

**6. Social Proof / Case Study**
- Specific testimonials with real names, roles, and results
- "Cut onboarding time from 3 weeks to 2 days" > "Great product!"

**7. Feature Deep-Dive (Optional)**
- Only if the audience is solution-aware and comparing options
- Keep it scannable: icon + heading + one line per feature

**8. Trust & Risk Reversal**
- Guarantees, security badges, certifications, compliance logos
- Free trial or money-back guarantee language

**9. FAQ**
- Address the top 3-5 objections identified in Step 3
- Direct, honest answers. No fluff.

**10. Final CTA**
- Restate the value prop from a different angle
- Repeat the same CTA button

### Step 5: Apply Copy Principles

Review the draft against these rules:
- [ ] Every sentence is about the customer, not the company
- [ ] Headline passes the 5-second test (visitor understands the offer instantly)
- [ ] CTA appears at least 3 times on the page
- [ ] Specific numbers and results used wherever possible
- [ ] No jargon the target audience wouldn't use
- [ ] Copy uses the customer's own language (from reviews, support tickets, interviews)
- [ ] Every section earns the next scroll

### Step 6: Deliver

Present the complete landing page copy in a structured format with clear section headings. Include:
- Copy for every section
- Visual/image direction notes in italics
- CTA button text called out explicitly
- Any notes on mobile-specific considerations

---

## Review Mode

### Step 2: Capture the Page

If given a URL, use WebFetch to retrieve the page content. If given a file or pasted content, read it directly.

### Step 3: Analyze Against Framework

Score the page on each dimension. See [references/page-anatomy.md](references/page-anatomy.md) for the full criteria.

**Strategy**
- Is there a single, clear value proposition?
- Is there one primary CTA (not competing goals)?
- Does the page match the likely visitor awareness level?

**Structure**
- Does it follow the proven section order? (Hero → Proof → Pain → Benefits → How it works → Deeper proof → Trust → FAQ → Final CTA)
- Is there a CTA above the fold?
- Is navigation removed or minimized?

**Copy**
- Does the headline pass the 5-second test?
- Is copy benefit-focused (not feature-focused)?
- Is it specific (numbers, results) rather than vague?
- Does it use customer language?
- Are objections addressed?

**Social Proof**
- Are testimonials specific with real names and results?
- Is proof placed early (not buried at the bottom)?
- Are trust badges, guarantees, or risk reversal present?

**Design Signals** (from the markup/structure)
- Is there clear visual hierarchy?
- Is the CTA high-contrast and repeated?
- Are form fields minimal?
- Is the page mobile-friendly?

### Step 4: Deliver the Review

Present findings as:

1. **Overall Assessment** — One paragraph summary. Is this page likely to convert well? What's the biggest issue?
2. **What's Working** — 2-3 things the page does well. Be specific.
3. **Critical Issues** — Problems that are likely hurting conversions right now. Prioritize by impact.
4. **Quick Wins** — Changes that would improve the page with minimal effort.
5. **Rewrite Suggestions** — For the headline, CTA, and any weak sections, provide specific alternative copy.

---

## Key References

- [Page Anatomy & Structure](references/page-anatomy.md) — Full breakdown of every section
- [Copywriting Frameworks](references/copywriting-frameworks.md) — PAS, PPPP, PRESTO with examples
- [Award-Winning Examples](references/examples.md) — Inspiration from top-performing landing pages

## Next Steps

After creating a landing page, suggest:
- `/content-writer` to write supporting content (ads, emails, social posts that drive traffic to the page)
- `/positioning` if the value proposition felt weak during creation
- Running A/B tests on the headline and CTA as the highest-leverage optimizations
