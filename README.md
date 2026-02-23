# bench-skills

A team of 25 reusable agent skills for product development, engineering, design, marketing, and operations. Works across Claude Code, Cursor, Copilot, and 35+ AI coding agents.

## Install

```bash
# Install all skills globally
npx skills add elliottjacobs/bench-skills -g

# Install specific skills
npx skills add elliottjacobs/bench-skills -g -s engineer-plan engineer-work

# List available skills
npx skills add elliottjacobs/bench-skills --list
```

## Skills

### Discovery & Strategy

| Skill | Command | Description |
|-------|---------|-------------|
| `client-discovery` | `/client-discovery` | Customer and market discovery using JTBD methodology, opportunity mapping, and PMF measurement |
| `positioning` | `/positioning` | Define product positioning, competitive frame, and strategic narrative |
| `pricing-packaging` | `/pricing-packaging` | Design pricing models, packaging tiers, and monetization strategy |
| `gtm-strategist` | `/gtm-strategist` | Build go-to-market plans with channel strategy, launch sequencing, and growth model |
| `ai-product-advisor` | `/ai-product-advisor` | Advise on AI-powered products — architecture, evals, UX, pricing, and GTM |

### Product

| Skill | Command | Description |
|-------|---------|-------------|
| `product-brainstorm` | `/product-brainstorm` | Guided requirements exploration through structured dialogue |
| `product-naming` | `/product-naming` | Expert naming process based on David Placek's methodology |
| `product-prd` | `/product-prd` | Write product requirements documents from feature ideas or brainstorm output |
| `product-tech-spec` | `/product-tech-spec` | Write technical specifications bridging product requirements and implementation |

### Engineering

| Skill | Command | Description |
|-------|---------|-------------|
| `engineer-plan` | `/engineer-plan` | Create structured implementation plans with parallel research agents |
| `engineer-plan-review` | `/engineer-plan-review` | Review plans for overengineering, complexity, and YAGNI violations |
| `engineer-work` | `/engineer-work` | Execute plans task-by-task with git branching, quality checks, and PR creation |
| `engineer-review` | `/engineer-review` | Multi-agent code review with parallel specialist reviewers |
| `security-audit` | `/security-audit` | Deep security audit with parallel domain-focused agents |

### Design

| Skill | Command | Description |
|-------|---------|-------------|
| `studio-brand-strategist` | `/studio-brand-strategist` | Brand strategy, voice, messaging framework, and competitive analysis |
| `design-lead` | `/design-lead` | Set design direction, review quality, and establish design tenets |
| `design-system` | `/design-system` | Build and maintain design systems — tokens, components, documentation, and governance |

### Marketing & Sales

| Skill | Command | Description |
|-------|---------|-------------|
| `content-writer` | `/content-writer` | Write marketing content — social posts, ad copy, emails, blog posts, case studies |
| `landing-page` | `/landing-page` | Create high-converting landing pages or review existing ones |
| `sales-pitch` | `/sales-pitch` | Build sales pitches, cold emails, demo scripts, and pitch decks |
| `proposal-writer` | `/proposal-writer` | Write persuasive client proposals and SOWs that close deals |

### Operations

| Skill | Command | Description |
|-------|---------|-------------|
| `onboarding` | `/onboarding` | Gather project and client context that all other skills reference |
| `bench-new-skill` | `/bench-new-skill` | Create new skills for this repo following all conventions |
| `knowledge-compound` | `/knowledge-compound` | Document solutions and learnings into a searchable knowledge base |
| `visualize` | `/visualize` | Generate interactive HTML pages with diagrams, tables, and visual explanations |

## Workflow

```
  DISCOVERY             PRODUCT               ENGINEERING            MARKETING
  /client-discovery     /product-brainstorm    /engineer-plan         /content-writer
        |                     |                      |                      |
        v                     v                      v                      v
  /positioning          /product-prd           /engineer-plan-review  /landing-page
        |                     |                      |
        v                     v                      v
  /pricing-packaging    /product-tech-spec     /engineer-work         /sales-pitch
        |                                            |                      |
        v                                            v                      v
  /gtm-strategist ──────────────────────────> /engineer-review       /proposal-writer
                                                     |
                                                     v
  DESIGN                                       /security-audit
  /studio-brand-strategist                           |
        |                                            v
        v                                      /knowledge-compound
  /design-lead
        |
        v
  /design-system
```

Start any new project with `/onboarding` to gather context that all other skills read.

## License

MIT
