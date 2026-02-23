# Design System Methodology

Distilled frameworks for building, governing, and scaling design systems. Sources: Brad Frost (Atomic Design), Figma Design Systems 101/102, Schema 2025, designsystems.com, Spotify/Atlassian/GitHub/Salesforce case studies.

---

## What Is a Design System

A design system is an organized collection of reusable standards, components, and patterns that teams use to build products consistently and efficiently.

**Layers (bottom → top):**
1. **Design tokens** — The smallest decisions: colors, typography, spacing, motion
2. **Components** — Reusable UI building blocks composed from tokens
3. **Patterns** — Common arrangements of components solving recurring problems
4. **Documentation** — Usage guidelines, principles, contribution rules

A design system is NOT just a component library. The library is one artifact; the system includes the tokens, documentation, governance, and culture around it.

**Benefits:**
- Consistency across products and teams
- Speed — stop redesigning solved problems
- Quality — bake accessibility and best practices into the system once
- Shared vocabulary between design and engineering
- Scalability — onboard new teams/products faster

---

## Atomic Design — 5 Levels

Brad Frost's mental model for component hierarchy:

| Level | What | Examples |
|-------|------|----------|
| **Atoms** | Smallest indivisible elements | Button, Input, Label, Icon, Badge |
| **Molecules** | Small groups of atoms functioning together | Search bar (input + button), Form field (label + input + error) |
| **Organisms** | Complex UI sections from molecules/atoms | Navigation header, Product card grid, Comment thread |
| **Templates** | Page-level layouts with placeholder content | Dashboard layout, Settings page layout |
| **Pages** | Templates filled with real content | Actual dashboard with live data |

**Core principle:** Build systems, not pages. Design the atoms and molecules; pages assemble themselves.

**When this model helps:**
- Deciding component granularity (is this an atom or molecule?)
- Planning what to build first (atoms before organisms)
- Structuring documentation (organize by level)

**When to deviate:**
- Not every team needs all 5 levels explicitly labeled
- The naming is a mental model, not a rigid taxonomy

---

## When to Build a Design System

**Signals you need one:**
- 3+ designers or engineers duplicating work
- Visual inconsistency across screens or products
- Accessibility gaps surfacing repeatedly
- Onboarding takes too long because there's no source of truth
- Design-to-dev handoff causes friction and misinterpretation
- Multiple products or brands sharing a common platform

**Maturity model:**

| Stage | Characteristics | What to do |
|-------|----------------|------------|
| **0 — Nothing** | No shared styles, everyone improvises | Start with tokens + 3-5 core components |
| **1 — Scattered** | Some shared colors/fonts, no structure | Audit what exists, formalize into tokens |
| **2 — Partial** | Component library exists, inconsistent use | Add governance, fill gaps, improve docs |
| **3 — Adopted** | Teams actively use the system | Measure adoption, optimize contribution flow |
| **4 — Scaled** | Multi-brand/theme, automated governance | Focus on tooling, automation, deprecation |

---

## Minimum Viable Design System

Don't try to build everything. Start with the smallest system that enables consistent building:

**Phase 1 — Foundations (week 1-2):**
- Color tokens (3 tiers: global → semantic → component)
- Typography tokens (scale + semantic styles)
- Spacing tokens (base unit + scale)

**Phase 2 — Core components (week 2-4):**
- Button (most used, establishes the pattern for all component specs)
- Input (forms are everywhere)
- Card (content container pattern)
- Badge/Tag (status communication)
- Modal/Dialog (overlay pattern)

**Phase 3 — Documentation + governance (week 4-6):**
- Getting started guide
- Token reference
- Component catalog
- Contribution process

**Phase 4 — Scale (ongoing):**
- Additional components as needed
- Multi-brand/theme support
- Automated linting and consistency checks
- Adoption measurement

---

## Component Lifecycle

Every component moves through a defined lifecycle:

```
Propose → Review → Build → Document → Release → Measure → Deprecate
```

| Phase | Who | What happens |
|-------|-----|-------------|
| **Propose** | Anyone | Submit a request with use case, rationale, and examples |
| **Review** | DS team / reviewers | Evaluate: Is this a new component or variant of existing? Does it serve multiple use cases? |
| **Build** | Designer + Engineer | Design the component, define tokens, build and test |
| **Document** | Builder | Write usage guidelines, do/don't examples, accessibility notes |
| **Release** | DS team | Version, changelog, announce to consumers |
| **Measure** | DS team | Track adoption, collect feedback, identify issues |
| **Deprecate** | DS team | When replaced or no longer needed — migration path required |

---

## Governance Models

| Model | How it works | Best for | Risk |
|-------|-------------|----------|------|
| **Centralized** | Dedicated DS team owns everything | Large orgs, strict consistency requirements | Bottleneck, slow to respond to team needs |
| **Federated** | Teams contribute, DS team reviews and maintains | Mid-size orgs, multiple product teams | Quality variance, coordination overhead |
| **Community** | Open contribution with guidelines, light review | Small orgs, high-trust teams | Drift, inconsistency without strong guidelines |

**Recommendation:** Most teams should start centralized (or single-owner) and move toward federated as the system matures and adoption grows.

---

## Adoption & Measurement

A design system that isn't adopted is just documentation nobody reads.

**Health metrics:**
- **Component coverage** — What % of UI is built with DS components vs. custom?
- **Token adoption** — Are teams using tokens or hardcoding values?
- **Override rate** — How often do teams override or work around the system?
- **Contribution rate** — Are teams proposing components or just consuming?
- **Time to productive** — How fast can a new team member build with the system?
- **Bug/inconsistency reports** — Decreasing over time = healthy system

**Adoption tactics:**
- Make the system the path of least resistance (easy to use, hard to bypass)
- Document with real examples, not abstract theory
- Provide starter templates and boilerplate
- Celebrate teams that contribute back
- Regular office hours or review sessions

---

## Common Anti-Patterns

| Anti-pattern | Why it fails |
|-------------|-------------|
| **Build everything first** | Teams wait too long, lose momentum, ship nothing |
| **No semantic layer** | Hardcoded `blue-500` everywhere — impossible to theme or rebrand |
| **Component-only thinking** | Skip tokens → components have inconsistent foundations |
| **No governance** | System drifts, forks appear, trust erodes |
| **Perfection over progress** | Waiting for the perfect system means never shipping |
| **Copy-paste from Material/Ant** | Borrowed systems don't reflect your brand or needs |
| **No documentation** | A component without docs is invisible |
| **One person's side project** | DS needs organizational commitment, not a hero |
| **Ignoring accessibility** | Retrofitting a11y is 10x harder than building it in |
| **Over-tokenizing** | Not every value needs a token — tokenize decisions, not pixels |
