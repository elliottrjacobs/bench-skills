# bench-skills

29 reusable agent skills for product development, engineering, design, GTM, marketing, and sales. Works across Claude Code, Cursor, Copilot, and 35+ AI coding agents.

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

### Onboarding

| Skill | Command | Description |
|-------|---------|-------------|
| `onboarding` | `/onboarding` | Smart project intake — detects engagement type (GTM, Product, Design/Brand, Engineering, General), gathers context, produces readiness scorecards and type-specific context files |

### GTM (Go-to-Market)

| Skill | Command | Description |
|-------|---------|-------------|
| `gtm-icp` | `/gtm-icp` | Build deep ICP and buyer persona profiles with buying committees, triggers, and language banks |
| `gtm-competitive-intel` | `/gtm-competitive-intel` | Competitive intelligence dossiers with GTM analysis, battlecards, and market dynamics |
| `gtm-positioning` | `/gtm-positioning` | Product positioning, messaging matrix, strategic narrative, and category strategy |
| `gtm-pricing` | `/gtm-pricing` | Pricing models, deal desk guidance, expansion revenue, and pricing communication |
| `gtm-strategy` | `/gtm-strategy` | Comprehensive GTM plans with growth model, channels, metrics, team model, and operating rhythm |
| `gtm-outreach` | `/gtm-outreach` | Multi-channel outreach sequences, prospecting strategy, and reply handling playbooks |
| `gtm-marketing` | `/gtm-marketing` | Content strategy by funnel stage, distribution plans, SEO/AEO, and content calendars |
| `gtm-meeting-prep` | `/gtm-meeting-prep` | Prepare for sales/investor/partnership meetings with account research and tailored briefs |

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
| `brand-strategist` | `/brand-strategist` | Brand strategy, voice, messaging framework, and competitive analysis |
| `design-lead` | `/design-lead` | Set design direction, review quality, and establish design tenets |
| `design-system` | `/design-system` | Build and maintain design systems — tokens, components, documentation, and governance |

### Content & Sales

| Skill | Command | Description |
|-------|---------|-------------|
| `content-writer` | `/content-writer` | Write marketing content — social posts, ad copy, emails, blog posts, case studies |
| `sales-pitch` | `/sales-pitch` | Build sales pitches, cold emails, demo scripts, and pitch decks |
| `proposal-writer` | `/proposal-writer` | Write persuasive client proposals and SOWs that close deals |

### Discovery

| Skill | Command | Description |
|-------|---------|-------------|
| `client-discovery` | `/client-discovery` | Customer and market discovery using JTBD methodology, opportunity mapping, and PMF measurement |

### Utility

| Skill | Command | Description |
|-------|---------|-------------|
| `create-new-skill` | `/create-new-skill` | Create new skills following all conventions |
| `knowledge-compound` | `/knowledge-compound` | Document solutions and learnings into a searchable knowledge base |
| `visualize` | `/visualize` | Generate interactive HTML pages with diagrams, tables, and visual explanations |
| `image-gen` | `/image-gen` | Generate images using AI APIs from the terminal |

## Workflow

```
  START HERE
  /onboarding ──────────────────────────────────────────────────────────┐
       │                                                                │
       ├── GTM engagement ──────────────────────────────────────────┐   │
       │   /gtm-icp → /gtm-competitive-intel → /gtm-positioning    │   │
       │        │              │                      │             │   │
       │        v              v                      v             │   │
       │   /gtm-strategy → /gtm-pricing → /gtm-outreach            │   │
       │        │                              │                    │   │
       │        v                              v                    │   │
       │   /gtm-marketing              /gtm-meeting-prep           │   │
       │                                                            │   │
       ├── Product engagement ──────────────────────────────────┐   │   │
       │   /client-discovery → /product-brainstorm              │   │   │
       │        │                     │                         │   │   │
       │        v                     v                         │   │   │
       │   /product-prd → /product-tech-spec → /engineer-plan   │   │   │
       │                                                        │   │   │
       ├── Design/Brand engagement ─────────────────────────┐   │   │   │
       │   /brand-strategist → /design-lead → /design-system│   │   │   │
       │                                                    │   │   │   │
       ├── Engineering engagement ──────────────────────┐   │   │   │   │
       │   /engineer-plan → /engineer-work              │   │   │   │   │
       │        │                  │                    │   │   │   │   │
       │        v                  v                    │   │   │   │   │
       │   /engineer-review → /security-audit           │   │   │   │   │
       │                                                │   │   │   │   │
       └── Sales (any) ─────────────────────────────────┘   │   │   │   │
           /content-writer  /sales-pitch  /proposal-writer  │   │   │   │
                                                            │   │   │   │
  ALWAYS AVAILABLE                                          │   │   │   │
  /knowledge-compound  /visualize  /image-gen               │   │   │   │
```

Start any new project with `/onboarding` to gather context that all other skills read.

## License

MIT
