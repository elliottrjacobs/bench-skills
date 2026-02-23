# Anthropic Guide Insights

Key insights from Anthropic's [Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf).

## Use Case Definition

Before writing anything, define 2-3 concrete use cases in this format:

```
Use Case: [Name]
Trigger: User says "[phrase]" or "[phrase]"
Steps:
1. [Step]
2. [Step]
3. [Step]
Result: [What the user gets]
```

Ask yourself:
- What does a user want to accomplish?
- What multi-step workflows does this require?
- Which tools are needed (built-in or MCP)?
- What domain knowledge or best practices should be embedded?

## Three Skill Categories

### Category 1: Document & Asset Creation
Creating consistent, high-quality output (documents, apps, designs, code).
- Embedded style guides and brand standards
- Template structures for consistent output
- Quality checklists before finalizing
- No external tools required — uses Claude's built-in capabilities

### Category 2: Workflow Automation
Multi-step processes that benefit from consistent methodology.
- Step-by-step workflow with validation gates
- Templates for common structures
- Built-in review and improvement suggestions
- Iterative refinement loops

### Category 3: MCP Enhancement
Workflow guidance to enhance tool access an MCP server provides.
- Coordinates multiple MCP calls in sequence
- Embeds domain expertise
- Provides context users would otherwise need to specify
- Error handling for common MCP issues

## Description Field Formula

```
[What it does] + [When to use it] + [Key capabilities]
```

Description must:
- Include BOTH what the skill does AND when to use it (trigger conditions)
- Be under 1024 characters
- Contain NO XML tags (< or >)
- Include specific tasks users might say
- Mention file types if relevant

Good examples:
```yaml
# Good — specific and actionable
description: Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for "design specs", "component documentation", or "design-to-code handoff".

# Good — includes trigger phrases
description: Manages Linear project workflows including sprint planning, task creation, and status tracking. Use when user mentions "sprint", "Linear tasks", "project planning", or asks to "create tickets".
```

Bad examples:
```yaml
# Too vague
description: Helps with projects.

# Missing triggers
description: Creates sophisticated multi-page documentation systems.

# Too technical, no user triggers
description: Implements the Project entity model with hierarchical relationships.
```

## Problem-First vs Tool-First

Choose your skill's framing:

- **Problem-first**: "I need to set up a project workspace" — Your skill orchestrates the right tool calls in the right sequence. Users describe outcomes; the skill handles the tools.
- **Tool-first**: "I have Notion MCP connected" — Your skill teaches Claude the optimal workflows and best practices. Users have access; the skill provides expertise.

## Five Proven Patterns

### Pattern 1: Sequential Workflow Orchestration
**Use when:** Multi-step processes in a specific order.
- Explicit step ordering
- Dependencies between steps
- Validation at each stage
- Rollback instructions for failures

### Pattern 2: Multi-MCP Coordination
**Use when:** Workflows span multiple services.
- Clear phase separation
- Data passing between MCPs
- Validation before moving to next phase
- Centralized error handling

### Pattern 3: Iterative Refinement
**Use when:** Output quality improves with iteration.
- Explicit quality criteria
- Iterative improvement
- Validation scripts
- Know when to stop iterating

### Pattern 4: Context-Aware Tool Selection
**Use when:** Same outcome, different tools depending on context.
- Clear decision criteria
- Fallback options
- Transparency about choices

### Pattern 5: Domain-Specific Intelligence
**Use when:** Your skill adds specialized knowledge beyond tool access.
- Domain expertise embedded in logic
- Compliance before action
- Comprehensive documentation
- Clear governance

## Success Criteria

### Quantitative Metrics
- Skill triggers on 90% of relevant queries
- Completes workflow in X tool calls (compare with/without skill)
- 0 failed API calls per workflow

### Qualitative Metrics
- Users don't need to prompt Claude about next steps
- Workflows complete without user correction
- Consistent results across sessions

## Testing Approach

### 1. Triggering Tests
Ensure your skill loads at the right times.
- Triggers on obvious tasks
- Triggers on paraphrased requests
- Doesn't trigger on unrelated topics

### 2. Functional Tests
Verify the skill produces correct outputs.
- Valid outputs generated
- API calls succeed
- Error cases covered
- Edge cases handled

### 3. Performance Comparison
Prove the skill improves results vs. baseline.

**Pro Tip:** Iterate on a single task before expanding. The most effective skill creators iterate on a single challenging task until Claude succeeds, then extract the winning approach into a skill.

## Best Practices for Instructions

### Be Specific and Actionable
```
# Good
Run `python scripts/validate.py --input {filename}` to check data format.
If validation fails, common issues include:
- Missing required fields (add them to the CSV)
- Invalid date formats (use YYYY-MM-DD)

# Bad
Validate the data before proceeding.
```

### Combat Model "Laziness"
Add explicit encouragement:
```markdown
## Performance Notes
- Take your time to do this thoroughly
- Quality is more important than speed
- Do not skip validation steps
```

### Avoid Ambiguous Language
```
# Bad
Make sure to validate things properly

# Good
CRITICAL: Before calling create_project, verify:
- Project name is non-empty
- At least one team member assigned
- Start date is not in the past
```

## Critical Rules

- **SKILL.md naming**: Must be exactly `SKILL.md` (case-sensitive). No variations.
- **Folder naming**: kebab-case only. No spaces, underscores, or capitals.
- **No README.md** inside skill folder. All documentation goes in SKILL.md or references/.
- **No XML tags** (< or >) in frontmatter — could inject into system prompt.
- **No "claude" or "anthropic"** in skill name (reserved).
- **Keep SKILL.md under 5,000 words**. Move detailed docs to references/.

## Troubleshooting

### Skill Doesn't Trigger
- Is the description too generic? ("Helps with projects" won't work)
- Does it include trigger phrases users would actually say?
- Does it mention relevant file types if applicable?
- **Debug**: Ask Claude "When would you use the [skill name] skill?" — it will quote the description back. Adjust based on what's missing.

### Skill Triggers Too Often
1. Add negative triggers: `Do NOT use for simple data exploration (use data-viz skill instead).`
2. Be more specific: `Processes PDF legal documents for contract review` not `Processes documents`
3. Clarify scope: `Use specifically for online payment workflows, not for general financial queries.`

### Instructions Not Followed
1. **Too verbose** — Keep concise, use bullet points and numbered lists
2. **Buried** — Put critical instructions at the top, use `## Important` or `## Critical` headers
3. **Ambiguous** — Be explicit about what to verify before each action

### Large Context Issues
- Move detailed docs to references/
- Link to references instead of inline
- Keep SKILL.md under 5,000 words

## Iteration Signals

### Undertriggering
- Skill doesn't load when it should
- Users manually enabling it
- Support questions about when to use it
- **Fix**: Add more detail and nuance to the description, especially keywords for technical terms

### Overtriggering
- Skill loads for irrelevant queries
- Users disabling it
- Confusion about purpose
- **Fix**: Add negative triggers, be more specific

## Quick Checklist (Reference A)

### Before You Start
- [ ] Identified 2-3 concrete use cases
- [ ] Tools identified (built-in or MCP)
- [ ] Reviewed guide and example skills
- [ ] Planned folder structure

### During Development
- [ ] Folder named in kebab-case
- [ ] SKILL.md file exists (exact spelling)
- [ ] YAML frontmatter has `---` delimiters
- [ ] name field: kebab-case, no spaces, no capitals
- [ ] description includes WHAT and WHEN
- [ ] No XML tags (< >) anywhere
- [ ] Instructions are clear and actionable
- [ ] Error handling included
- [ ] Examples provided
- [ ] References clearly linked

### Before Upload/Install
- [ ] Tested triggering on obvious tasks
- [ ] Tested triggering on paraphrased requests
- [ ] Verified doesn't trigger on unrelated topics
- [ ] Functional tests pass
- [ ] Tool integration works (if applicable)

### After Install
- [ ] Test in real conversations
- [ ] Monitor for under/over-triggering
- [ ] Collect user feedback
- [ ] Iterate on description and instructions
