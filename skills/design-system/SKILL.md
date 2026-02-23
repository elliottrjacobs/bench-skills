---
name: design-system
description: Build and maintain design systems — tokens, components, documentation, and governance. Use when the user says "design system", "component library", "design tokens", "build a design system", "token architecture", or needs to create or audit a design system for a product or organization.
allowed-tools: ["Read", "Glob", "Grep", "WebSearch", "AskUserQuestion", "Write"]
---

# /design-system — Design System Builder

Build design system artifacts — token definitions, component specs, documentation, and governance models. Produces real, usable output, not just strategy.

## When to Use

- User says "design system", "design tokens", "component library", "token architecture"
- Building a design system from scratch for a new product
- Auditing or extending an existing design system
- Creating token definitions (color, typography, spacing, etc.)
- Speccing components (props, variants, states, accessibility)
- Setting up multi-brand or multi-theme token architecture

## Before Starting

Check for existing context:
1. Read `projects/<project>/onboarding.md` for project context
2. Read `projects/<project>/positioning.md` for brand personality
3. Read `projects/<project>/design-direction.md` for design tenets and vision
4. Check `docs/strategy/` for brand strategy work
5. Check for any existing design system files in the project

Design tenets and vision come from `/design-lead`. This skill builds the infrastructure, not the philosophy.

## Process

### Step 1: Intake

If project context exists, use it. Otherwise, gather basics:
- What product/brand is this for?
- Any existing brand colors, typography choices, or assets?

Then determine scope:

```
AskUserQuestion:
  question: "Where are you with your design system?"
  header: "Stage"
  options:
    - label: "Starting fresh"
      description: "No design system exists — building from scratch"
    - label: "Scattered styles"
      description: "Some colors/fonts defined but no organized system"
    - label: "Partial system"
      description: "Have some tokens or components, need to formalize and extend"
    - label: "Multi-brand"
      description: "Need to support multiple brands or themes from one system"
```

Then determine what to build:

```
AskUserQuestion:
  question: "What scope makes sense right now?"
  header: "Scope"
  options:
    - label: "Minimum viable DS (Recommended)"
      description: "Core tokens (color, type, spacing) + 3-5 key components — enough to start building consistently"
    - label: "Full foundations"
      description: "Complete token architecture across all categories, no components yet"
    - label: "Full system"
      description: "Foundations + 10+ component specs + documentation + governance"
```

### Step 2: Audit (if existing system)

If the user has existing styles, tokens, or components — audit first:

- **What exists today?** Scan for defined colors, fonts, spacing values, components
- **What's inconsistent?** Duplicate values, naming drift, undocumented patterns
- **What's missing?** Gaps in the token tiers, unspec'd components, no semantic layer
- **Adoption issues?** Are people using the system or working around it?

Present audit findings and confirm priorities before building.

### Step 3: Foundations — Token Architecture

See [references/token-architecture.md](references/token-architecture.md) for detailed naming conventions, tier model, and W3C format.

Before generating tokens, ask:

```
AskUserQuestion:
  question: "What output format do you need for tokens?"
  header: "Format"
  options:
    - label: "Documentation only (Recommended)"
      description: "Structured markdown with names, values, and usage guidance"
    - label: "CSS custom properties"
      description: "Ready-to-use --token-name: value declarations"
    - label: "JSON (W3C Design Tokens)"
      description: "Interoperable JSON following the W3C standard"
    - label: "Tailwind config"
      description: "Token values mapped to Tailwind theme configuration"
```

Generate token definitions across foundation layers:

**Color tokens (3 tiers):**
- Global: raw palette (`color.blue.500: #3B82F6`)
- Semantic: purpose-mapped (`color.primary: {color.blue.500}`, `color.error: {color.red.500}`)
- Component: scoped (`button.primary.bg: {color.primary}`)

**Typography tokens:**
- Font families, weights, size scale, line heights, letter spacing
- Semantic type styles (`heading.lg`, `body.md`, `label.sm`)

**Spacing tokens:**
- Base unit + scale (e.g., 4px base: `space.1: 4px`, `space.2: 8px`, ... `space.8: 32px`)
- Semantic spacing (`spacing.inline: {space.2}`, `spacing.stack: {space.4}`)

**Other foundations:**
- Border radius, elevation/shadow, motion/duration, breakpoints, z-index, opacity

**Accessibility foundations:**
- Color contrast targets (AA minimum, AAA where feasible) — document which token pairings meet which level
- Focus ring tokens (width, color, offset) — consistent, visible focus indicators
- Motion tokens with reduced-motion alternatives (`duration.normal: 200ms`, `duration.reduced: 0ms`)
- Minimum touch/click target sizes (44px iOS, 48px Android/Material)

**Multi-brand/theme (if applicable):**
- Mode structure (light/dark, brand A/B)
- Which tokens are brandable vs. fixed
- Override strategy: core system + brand layer

### Step 4: Component Architecture

For each component, generate a spec:

- **Name** and atomic level (atom / molecule / organism)
- **Purpose** — what job this component does
- **Anatomy** — parts breakdown
- **Props/API** — configurable properties (size, variant, state, disabled, etc.)
- **Variants** — all visual variations
- **States** — default, hover, active, focus, disabled, loading, error
- **Accessibility** — ARIA roles, keyboard behavior, screen reader text
- **Token mapping** — which tokens this component consumes
- **Composition** — what it's built from, what it composes into
- **Do/Don't** — usage guidelines

Confirm which components to spec:

```
AskUserQuestion:
  question: "Which components should we spec first?"
  header: "Components"
  multiSelect: true
  options:
    - label: "Core inputs"
      description: "Button, Input, Select, Checkbox, Toggle"
    - label: "Content display"
      description: "Card, Badge/Tag, Avatar, Table/List"
    - label: "Feedback & overlay"
      description: "Modal/Dialog, Toast/Alert, Tooltip, Dropdown"
    - label: "Navigation"
      description: "Nav bar, Tabs, Breadcrumbs, Sidebar"
```

### Step 5: Documentation Structure

Generate a documentation outline covering:
- Getting started guide (installation, usage, contributing)
- Token reference (generated from Step 3)
- Component catalog (generated from Step 4)
- Pattern library (common compositions — forms, data display, navigation)
- Contribution guide (how to propose new components or changes)

### Step 6: Governance Model

Define how the system is maintained:

- **Governance model** — Centralized (DS team owns all), Federated (teams contribute with DS team review), or Community (open contribution with guidelines)
- **Component lifecycle** — Propose → Review → Build → Document → Release → Measure → Deprecate
- **Contribution process** — How to propose new components or token changes
- **Quality gates** — What must be true before a component ships (tokens used, a11y checked, documented, reviewed)
- **Adoption measurement** — How to track DS health (component coverage, token usage, overrides/workarounds)

### Step 7: Validate & Output

Present all artifacts for review:

```
AskUserQuestion:
  question: "Does this design system feel right? What needs adjustment before we save?"
  header: "Review"
  options:
    - label: "Looks good"
      description: "Save everything as-is"
    - label: "Adjust tokens"
      description: "Some token values or naming needs tweaking"
    - label: "Adjust components"
      description: "Component specs need changes"
    - label: "Scope change"
      description: "Need to add or remove sections"
```

Save to: `projects/<project>/design-system/`

```
design-system/
├── foundations/
│   ├── tokens.md          # All token definitions
│   ├── color.md           # Color system detail
│   ├── typography.md      # Type system detail
│   └── spacing.md         # Spacing system detail
├── components/
│   ├── button.md          # Component spec
│   ├── input.md
│   └── ...
├── patterns/
│   └── form.md            # Common patterns
├── governance.md          # Governance model
└── README.md              # Getting started
```

For smaller projects, offer a single-file option: `projects/<project>/design-system.md`

## Methodology

This skill synthesizes frameworks from leading design system practitioners. See [references/design-system-methodology.md](references/design-system-methodology.md) for detailed methodology and [references/token-architecture.md](references/token-architecture.md) for token naming and structure.

Key sources: Brad Frost (Atomic Design), Figma Design Systems 101/102, Schema 2025, W3C Design Tokens spec, designsystems.com case studies (Spotify, Atlassian, GitHub, Salesforce).

## Output

Save to: `projects/<project>/design-system/` (multi-file) or `projects/<project>/design-system.md` (single-file)

## Next Steps

- Need design philosophy first? → `/design-lead` for tenets and creative direction
- Ready to implement? → `/engineer-plan` to plan the build
- Need to propose DS work to a client? → `/proposal-writer`
- Need to name the system? → `/product-naming`
