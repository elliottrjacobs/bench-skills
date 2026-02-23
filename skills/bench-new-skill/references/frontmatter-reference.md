# Frontmatter Field Reference

Complete catalog of YAML frontmatter fields for SKILL.md files. All fields are optional. Only `description` is recommended.

Source: [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)

## Core Fields

### `name`
Display name and slash command identifier. If omitted, uses the directory name.
- Lowercase letters, numbers, and hyphens only
- Max 64 characters

```yaml
name: my-skill
```

### `description`
**Recommended.** Tells Claude when to use the skill. If omitted, uses the first paragraph of markdown content.

Focus on triggering conditions, not capabilities:
```yaml
# Good — tells Claude WHEN
description: Use when the user says "review this", "code review", or provides a PR number.

# Bad — tells Claude WHAT
description: A comprehensive code review tool that analyzes security and performance.
```

### `allowed-tools`
Tools Claude can use without per-use permission approval when this skill is active. Your permission settings still govern all other tools.

```yaml
allowed-tools: Read, Grep, Glob
# or as array:
allowed-tools: ["Read", "Grep", "Glob", "Bash"]
```

Common tool sets:
- **Read-only research**: `Read, Grep, Glob`
- **Code modification**: `Read, Write, Edit, Glob, Grep, Bash`
- **Full workflow**: `Read, Write, Edit, Glob, Grep, Bash, Task, TodoWrite, AskUserQuestion`

## Invocation Control

### `disable-model-invocation`
When `true`, only the user can invoke the skill via `/skill-name`. Claude cannot load it automatically.

Use for workflows with side effects: deploy, commit, send messages, create PRs.

```yaml
disable-model-invocation: true
```

### `user-invocable`
When `false`, hides the skill from the `/` menu. Only Claude can invoke it.

Use for background knowledge that isn't actionable as a command.

```yaml
user-invocable: false
```

### Invocation Matrix

| Setting | User invokes | Claude invokes | Context loading |
|---------|-------------|---------------|-----------------|
| Default | Yes | Yes | Description always loaded; full skill loads on invoke |
| `disable-model-invocation: true` | Yes | No | Description NOT loaded; full skill loads on user invoke |
| `user-invocable: false` | No | Yes | Description always loaded; full skill loads on invoke |

## Arguments

### `argument-hint`
Hint shown during autocomplete to indicate expected arguments.

```yaml
argument-hint: [issue-number]
# or:
argument-hint: [filename] [format]
```

### String Substitutions in Skill Content

These are used in the markdown body, not in frontmatter:

| Variable | Description |
|----------|-------------|
| `$ARGUMENTS` | All arguments as a string. If not present in content, appended as `ARGUMENTS: <value>` |
| `$ARGUMENTS[N]` | Access argument by 0-based index (`$ARGUMENTS[0]`, `$ARGUMENTS[1]`) |
| `$N` | Shorthand for `$ARGUMENTS[N]` (`$0`, `$1`, `$2`) |
| `${CLAUDE_SESSION_ID}` | Current session ID for logging or correlation |

Example:
```markdown
Fix GitHub issue $ARGUMENTS following our coding standards.
```

```markdown
Migrate the $0 component from $1 to $2.
```

## Execution Context

### `context`
Set to `fork` to run the skill in an isolated subagent. The skill content becomes the subagent's task prompt. The subagent has no access to conversation history.

```yaml
context: fork
```

Only use `context: fork` when the skill contains an explicit task. If it's just guidelines or conventions, the subagent won't have an actionable prompt and will return without meaningful output.

### `agent`
Which subagent type to use when `context: fork` is set. Options:
- `Explore` — Read-only codebase exploration (Glob, Grep, Read, WebFetch, WebSearch)
- `Plan` — Architecture and planning (all read tools, no write)
- `general-purpose` — Full tool access (default if omitted)
- Any custom agent name from `.claude/agents/`

```yaml
context: fork
agent: Explore
```

### `model`
Override the model used when this skill is active.

```yaml
model: sonnet
```

## Lifecycle

### `hooks`
Hooks scoped to this skill's lifecycle. Same format as hooks in settings.json.

```yaml
hooks:
  PreToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: "echo 'About to modify a file'"
```

See [Hooks documentation](https://code.claude.com/docs/en/hooks) for configuration format.

## Dynamic Context Injection

Not a frontmatter field, but used in the skill markdown body. The `!`command`` syntax runs shell commands before the skill content is sent to Claude:

```markdown
## Context
- PR diff: !`gh pr diff`
- Current branch: !`git branch --show-current`
- Changed files: !`git diff --name-only`
```

Each command executes immediately. The output replaces the placeholder. Claude only sees the final rendered content.

## Extended Thinking

Include the word "ultrathink" anywhere in your skill content to enable extended thinking mode.

## Skill Location Priority

When skills share the same name across levels, higher-priority locations win:

1. **Enterprise** (managed settings) — highest priority
2. **Personal** (`~/.claude/skills/`)
3. **Project** (`.claude/skills/`)
4. **Plugin** (`plugin-name:skill-name` — namespaced, so no conflicts)

Plugin skills use `plugin-name:skill-name` namespace and cannot conflict with other levels.

## Context Budget

Skill descriptions are loaded into context so Claude knows what's available. If you have many skills, they may exceed the character budget (2% of context window, fallback 16,000 chars). Run `/context` to check for excluded skills.

Override with: `SLASH_COMMAND_TOOL_CHAR_BUDGET` environment variable.
