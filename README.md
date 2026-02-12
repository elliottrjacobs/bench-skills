# bench-skills

A team of reusable agent skills for product development and engineering. Works across Claude Code, Cursor, Copilot, and 35+ AI coding agents.

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

### Engineering

| Skill | Command | Description |
|-------|---------|-------------|
| `engineer-plan` | `/engineer-plan` | Create structured implementation plans with parallel research agents |
| `engineer-work` | `/engineer-work` | Execute plans task-by-task with git branching, quality checks, and PR creation |
| `engineer-review` | `/engineer-review` | Multi-agent code review with 4-8 parallel specialist reviewers |

### Product

| Skill | Command | Description |
|-------|---------|-------------|
| `product-brainstorm` | `/product-brainstorm` | Guided requirements exploration through dialogue |
| `product-prd` | `/product-prd` | Write product requirements documents |
| `product-tech-spec` | `/product-tech-spec` | Write technical specifications with architecture decisions |
| `product-naming` | `/product-naming` | Expert naming process (Placek methodology) |

### Security

| Skill | Command | Description |
|-------|---------|-------------|
| `security-audit` | `/security-audit` | Deep security audit with parallel domain-focused agents |
| `security-supabase` | `/security-supabase` | Supabase security best practices — RLS, auth, API, storage, realtime, edge functions |

### Knowledge

| Skill | Command | Description |
|-------|---------|-------------|
| `knowledge-compound` | `/knowledge-compound` | Document solutions and learnings into a searchable knowledge base |

### Meta

| Skill | Command | Description |
|-------|---------|-------------|
| `bench-new-skill` | `/bench-new-skill` | Create new skills for this repo following all conventions |

## Workflow Pipeline

```
  PRODUCT                    ENGINEERING                POST-SHIP
  /product-brainstorm        /engineer-plan             /engineer-review
        |                          |                          |
        v                          v                          v
  /product-prd  -------->    /engineer-work  -------->  /knowledge-compound
        |                          |
        v                          v
  /product-tech-spec         /security-audit
                             /security-supabase
```

## License

MIT
