---
name: bench-new-skill
description: Create new skills for the bench-skills repo following all conventions. Use when the user says "create a new skill", "add a skill", "new slash command", or wants to extend bench-skills with additional capabilities.
allowed-tools: ["Read", "Glob", "Grep", "Write", "AskUserQuestion", "Bash"]
---

# /bench-new-skill — Create New Skills

Scaffold new skills for the bench-skills repo following established conventions and best practices.

## When to Use

- User wants to add a new skill to bench-skills
- User says "create a new skill", "add a skill", "new slash command"

## Principles

1. **Description = triggering conditions.** The description field tells Claude WHEN to use the skill, not WHAT it does. Focus on user phrases and situations that should trigger it.
2. **Start with concrete examples.** Ask "What would you say to trigger this skill?" before designing anything.
3. **Include only what Claude doesn't already know.** Don't teach Claude how to write code — teach it your specific process, conventions, and standards.
4. **Self-contained skills.** Each skill directory must work independently when installed via `npx skills add`.
5. **Inline agent prompts.** Keep agent prompts to 5-15 lines inline in SKILL.md. Only use `references/` for large templates or catalogs.
6. **Under 300 lines.** If SKILL.md exceeds 300 lines, move large content to `references/`.

## Process

### Step 1: Understand the Skill

Ask the user:

```
AskUserQuestion:
  question: "What should this skill do? Give me 2-3 examples of what you'd say to trigger it."
```

From their response, identify:
- **Trigger phrases** — What the user would say to invoke this
- **Core process** — What steps the skill should follow
- **Output** — What the skill produces
- **Agents needed** — Does it need parallel subagents? For what?

### Step 2: Name the Skill

Follow the `{area}-{name}` convention:

| Area | For |
|------|-----|
| `engineer-` | Engineering workflow (plan, build, review) |
| `product-` | Product management (specs, requirements) |
| `security-` | Security auditing and best practices |
| `knowledge-` | Documentation and learning capture |
| `bench-` | Meta skills for bench-skills itself |

Suggest a name and confirm with the user.

### Step 3: Write the Description

The description field is the most important part — it determines when Claude activates the skill.

**Good descriptions** (focus on triggers):
```
Use when the user says "review this", "code review", or provides a PR number.
```

**Bad descriptions** (focus on capabilities):
```
A comprehensive code review tool that analyzes security, performance, and architecture.
```

### Step 4: Scaffold the Skill

Create the skill directory in the bench-skills repo:

```
~/bench-skills/skills/{skill-name}/
├── SKILL.md
└── references/          (only if needed for large templates/catalogs)
    └── {reference}.md
```

Write SKILL.md with this structure:

```markdown
---
name: {skill-name}
description: {triggering conditions}
allowed-tools: [{tools needed}]
---

# /{skill-name} — {Short Title}

{One sentence explaining what this skill does.}

## When to Use

- {Trigger condition 1}
- {Trigger condition 2}
- {Trigger condition 3}

## Process

### Step 1: {First Step}
{Instructions}

### Step 2: {Second Step}
{Instructions}

...

## Output

{What the skill produces and where it's saved.}

## Next Steps

{What to do after running this skill. Link to other skills.}
```

### Step 5: Validate

Check the skill against conventions:
- [ ] Name follows `{area}-{name}` pattern
- [ ] Description focuses on triggering conditions
- [ ] SKILL.md under 300 lines
- [ ] Self-contained (no dependencies on shared files)
- [ ] Agent prompts are inline (5-15 lines each)
- [ ] Steps are numbered and actionable
- [ ] Links to other bench-skills where appropriate
- [ ] `allowed-tools` includes only tools actually used

### Step 6: Install

Tell the user to install the new skill:

```bash
npx skills add ~/bench-skills -g -s {skill-name}
```

Or if using symlink mode and the repo is already installed, it's automatically available.

## Skill Design Checklist

Before finalizing, verify:

1. **Would this be triggered naturally?** If the description requires users to know specific phrases, it's too narrow.
2. **Is this distinct from existing skills?** Check that it doesn't overlap with: engineer-plan, engineer-work, engineer-review, product-brainstorm, product-prd, product-tech-spec, product-naming, security-audit, security-supabase, knowledge-compound.
3. **Is this teaching Claude something new?** If Claude already knows how to do this without special instructions, the skill may not be needed.
4. **Is this the right granularity?** A skill should be a complete workflow, not a single step. But it shouldn't try to do everything either.
