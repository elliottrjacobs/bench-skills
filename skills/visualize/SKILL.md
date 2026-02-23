---
name: visualize
description: "Generate self-contained interactive HTML pages with professional diagrams, data tables, and visual explanations. Use when the user says 'visualize', 'create a diagram', 'draw this', 'map this out', 'show me visually', 'generate a visual', 'architecture diagram', 'flow diagram', or needs to explain something with interactive diagrams."
allowed-tools: [Read, Write, Edit, Bash, Glob, Grep, Task, AskUserQuestion]
argument-hint: [topic or file path]
---

# /visualize — Interactive Visual Explainer

Generate self-contained HTML pages with interactive Mermaid diagrams, styled data tables, and professional layouts. No build tools, no dependencies to install — just open in a browser.

## When to Use

- User says "visualize", "diagram", "draw this out", "map this", "show me visually"
- User wants architecture diagrams, user flows, data flows, state machines
- User wants to explain a system, process, or codebase visually
- User provides a topic, codebase, or document to turn into diagrams

## What It Produces

A single `.html` file with:
- Interactive Mermaid diagrams (flowcharts, sequence, state, ER, etc.)
- Zoom/pan on every diagram (buttons + scroll wheel + drag)
- Fixed sidebar with scroll-spy navigation
- Dark/light theme toggle
- Styled data tables and stat cards
- Mobile-responsive layout
- Zero external dependencies at runtime (Mermaid loaded from CDN)

## Process

### Step 1: Determine Scope

If `$ARGUMENTS` points to a file or directory, analyze that code.
If the user described a topic or system, use that as the subject.
If unclear, ask:

```
AskUserQuestion:
  question: "What should I visualize? A codebase, a system design, a process, or something else?"
```

### Step 2: Research the Subject

**For codebases** — launch parallel Task agents to explore:

1. **Structure agent**: Map routes, files, modules, entry points
2. **Interactions agent**: Map user actions, buttons, forms, navigation
3. **Data flow agent**: Map API calls, state management, mutations, caching

**For systems/processes** — gather information through conversation, web search, or file reading. Identify:
- Components and their relationships
- Flows (happy path, error paths, edge cases)
- Data: tables, entities, state transitions
- Decision points and branching logic

### Step 3: Plan the Diagrams

Organize findings into diagram sections. Choose the right Mermaid diagram type for each:

| Content | Diagram Type | Mermaid Syntax |
|---------|-------------|----------------|
| Navigation, user flows | Flowchart | `flowchart TD` or `flowchart LR` |
| Step-by-step processes | Flowchart | `flowchart TD` |
| API call sequences | Sequence diagram | `sequenceDiagram` |
| Entity relationships | ER diagram | `erDiagram` |
| State machines | State diagram | `stateDiagram-v2` |
| Timelines, phases | Timeline | `timeline` |
| Class/module structure | Class diagram | `classDiagram` |
| Git/branching flows | Gitgraph | `gitGraph` |

Group diagrams into logical sections with a sidebar nav hierarchy.

### Step 4: Build the HTML Page

Use the template from [references/html-template.md](references/html-template.md). This is the exact structure — follow it precisely:

1. **HTML skeleton** with Mermaid CDN, CSS variables, sidebar, main content area
2. **Sidebar** with scroll-spy navigation linking to each section
3. **Hero section** with title, description, and metadata pills
4. **Stat cards** (info-grid) for key metrics
5. **Diagram cards** — each diagram wrapped in a `.diagram-card` with header, badge, and `.diagram-body`
6. **Data tables** (`.table-wrap`) for supplementary detail
7. **Zoom/pan JS** injected into every diagram card automatically
8. **Theme toggle** with Mermaid re-rendering on switch

See [references/css-patterns.md](references/css-patterns.md) for the complete stylesheet.
See [references/js-interactions.md](references/js-interactions.md) for zoom/pan, scroll-spy, and theme toggle code.

### Step 5: Apply Visual Design

**Color-code diagram nodes** by role:

| Color | Hex | Meaning |
|-------|-----|---------|
| Green | `#dcfce7` / `#22c55e` | Entry points, main sections, success |
| Purple | `#ede9fe` / `#8b5cf6` | Detail screens, modal views |
| Blue | `#dbeafe` / `#3b82f6` | Mutations, API calls, data operations |
| Yellow | `#fef3c7` / `#f59e0b` | Warnings, cache invalidation, external |
| Red | `#fee2e2` / `#ef4444` | Auth gates, destructive actions, errors |

Always add `color:#000` to node styles for legibility.

**Card badges** — label each diagram card with its type: `Flowchart`, `Architecture`, `Data Flow`, `Sequence`, `State Machine`.

**Sidebar dots** — color-code nav items with dot indicators matching diagram colors.

### Step 6: Write the File

Save to a sensible location:
- If visualizing a project: `docs/architecture/{topic}-diagrams.html`
- If standalone: `{topic}-visual.html` in current directory

Open the file in the browser after writing:
```bash
open {file-path}
```

### Step 7: Iterate

Ask the user if they want to:
- Add more diagram sections
- Adjust the level of detail
- Change the visual style or layout
- Add specific data tables

## Diagram Writing Tips

- **Keep node labels short** — 3-5 words max. Use `<br/>` for line breaks, `<b>` for emphasis.
- **Use subgraphs** for grouping related nodes (especially in data flow diagrams).
- **Prefer TD (top-down) for hierarchies**, LR (left-right) for sequences and pipelines.
- **Dashed lines (`-.->`)** for optional/conditional paths, **solid lines (`-->`)** for primary paths.
- **Decision nodes** use `{curly braces}` for diamond shapes.
- **HTML entities** in labels: `&rarr;` for arrows, `&middot;` for dots, `&plusmn;` for plus/minus.
- **Max ~15-20 nodes per diagram** — split larger flows into multiple diagrams.
- **Always include a color legend** at the bottom of the page.

## Output

Single self-contained `.html` file. No build step. Opens in any browser. Includes:
- All CSS inlined in `<style>` tag
- All JS inlined in `<script>` tag
- Mermaid loaded from CDN (`https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js`)
