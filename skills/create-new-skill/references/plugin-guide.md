# Plugin Creation Guide

When a skill needs to be shared across projects or teams, package it as a plugin. Plugins bundle skills, agents, hooks, and MCP servers into a distributable unit.

Source: [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)

## When to Use a Plugin vs Standalone Skill

| Factor | Standalone Skill | Plugin |
|--------|-----------------|--------|
| Scope | Single project or personal use | Shared across projects/teams |
| Naming | `/skill-name` | `/plugin-name:skill-name` |
| Versioning | None | Semantic versioning |
| Distribution | Copy files manually | Install via marketplace or `--plugin-dir` |
| Bundling | Single skill only | Multiple skills + agents + hooks + MCP/LSP servers |

**Start standalone, convert to plugin when ready to share.**

## Plugin Directory Structure

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json        # Manifest (required)
├── skills/                 # Skills with SKILL.md files
│   └── code-review/
│       └── SKILL.md
├── commands/               # Legacy commands as .md files
│   └── hello.md
├── agents/                 # Custom agent definitions
│   └── my-agent.md
├── hooks/                  # Event handlers
│   └── hooks.json
├── .mcp.json               # MCP server configurations
└── .lsp.json               # LSP server configurations
```

**Common mistake:** Don't put `commands/`, `agents/`, `skills/`, or `hooks/` inside `.claude-plugin/`. Only `plugin.json` goes there.

## Plugin Manifest

Create `.claude-plugin/plugin.json`:

```json
{
  "name": "my-plugin",
  "description": "What this plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  }
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique identifier. Becomes the skill namespace prefix. |
| `description` | Yes | Shown in the plugin manager. |
| `version` | Yes | Semantic versioning (major.minor.patch). |
| `author` | No | Attribution. Has `name` subfield. |
| `homepage` | No | URL to project homepage. |
| `repository` | No | URL to source repository. |
| `license` | No | License identifier. |

## Adding Skills to a Plugin

Each skill is a directory under `skills/` containing a `SKILL.md`:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── review/
        └── SKILL.md
```

The skill becomes `/my-plugin:review`. SKILL.md format is identical to standalone skills — same frontmatter, same content.

## Adding Hooks to a Plugin

Create `hooks/hooks.json` at the plugin root. Format is the same as hooks in `settings.json`. The command receives hook input as JSON on stdin:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npm run lint:fix"
          }
        ]
      }
    ]
  }
}
```

## Adding MCP Servers

Create `.mcp.json` at the plugin root:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["path/to/server.js"]
    }
  }
}
```

## Adding LSP Servers

Create `.lsp.json` at the plugin root for code intelligence:

```json
{
  "go": {
    "command": "gopls",
    "args": ["serve"],
    "extensionToLanguage": {
      ".go": "go"
    }
  }
}
```

Users must have the language server binary installed.

## Testing Plugins

Load your plugin locally with `--plugin-dir`:

```bash
claude --plugin-dir ./my-plugin
```

Test checklist:
- [ ] Skills appear under `/plugin-name:skill-name`
- [ ] Agents appear in `/agents` (if any)
- [ ] Hooks trigger correctly (if any)
- [ ] MCP servers connect (if any)

Load multiple plugins:
```bash
claude --plugin-dir ./plugin-one --plugin-dir ./plugin-two
```

Restart Claude Code to pick up changes during development.

## Converting Standalone to Plugin

1. Create the plugin structure:
   ```bash
   mkdir -p my-plugin/.claude-plugin
   ```

2. Create the manifest (`my-plugin/.claude-plugin/plugin.json`)

3. Copy existing files:
   ```bash
   cp -r .claude/skills my-plugin/
   cp -r .claude/agents my-plugin/    # if any
   ```

4. Migrate hooks from `settings.json` to `my-plugin/hooks/hooks.json`

5. Test with `--plugin-dir`

6. Remove originals from `.claude/` to avoid duplicates (plugin takes precedence)

## Distributing Plugins

- **Team sharing:** Host in a git repo, install via marketplace configuration
- **Community:** Create or use a [plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
- **Enterprise:** Deploy through [managed settings](https://code.claude.com/docs/en/permissions#managed-settings)

Include a README.md with installation and usage instructions.
