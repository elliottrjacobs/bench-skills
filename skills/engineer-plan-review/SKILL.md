---
name: engineer-plan-review
description: Review implementation plans for overengineering, unnecessary complexity, and YAGNI violations. Use when the user says "review this plan", "simplify the plan", "is this plan overengineered", or after completing /engineer-plan and before /engineer-work.
allowed-tools: ["Read", "Glob", "Grep", "Task", "Write", "Edit", "AskUserQuestion"]
---

# /engineer-plan-review — Plan Simplicity Review

Review implementation plans through multiple simplification lenses to catch overengineering, YAGNI violations, and unnecessary complexity before any code is written.

## When to Use

- User says "review this plan", "check the plan", "simplify the plan"
- After completing `/engineer-plan` and before starting `/engineer-work`
- User suspects a plan is overengineered or too complex

## Process

### Step 1: Locate the Plan

Find the plan to review:
1. If `$ARGUMENTS` specifies a path, use that
2. Otherwise check `docs/plans/` for the most recent plan
3. If no plan found, ask the user

Read the full plan. Also read any referenced specs in `docs/tech-specs/` or `docs/prds/` to understand what was actually requested.

### Step 2: Launch Parallel Reviewers

Spawn ALL 4 reviewers IN PARALLEL using the Task tool. Send all Task calls in a single message.

Each reviewer receives the full plan content and any referenced spec content inline in their prompt.

#### YAGNI Reviewer
```
Task(subagent_type: "general-purpose", description: "YAGNI review of plan")
prompt: Review this implementation plan for YAGNI (You Aren't Gonna Need It) violations.

  [PASTE FULL PLAN HERE]

  [PASTE REFERENCED SPEC IF ANY]

  For each task in the plan, ask: "Is this required by the spec or is it speculative?"

  Flag tasks or subtasks that:
  - Build for hypothetical future requirements not in the spec
  - Add configurability or extensibility that isn't requested
  - Include abstractions for things that only have one concrete use
  - Add feature flags, toggle systems, or plugin architectures prematurely
  - Build admin tools, dashboards, or management UIs not in scope

  For each finding, return:
  - Task number and name
  - What's unnecessary and why
  - Suggested simplification (remove entirely, or reduce scope)
```

#### Complexity Reviewer
```
Task(subagent_type: "general-purpose", description: "Complexity review of plan")
prompt: Review this implementation plan for unnecessary complexity.

  [PASTE FULL PLAN HERE]

  [PASTE REFERENCED SPEC IF ANY]

  Look for:
  - Tasks that could be combined into simpler ones
  - Overly granular decomposition (tasks that are artificially split)
  - Unnecessary indirection layers (services wrapping services, adapters for single implementations)
  - Custom solutions where a library or built-in would suffice
  - Premature optimization tasks (caching, indexing, performance work before measuring)
  - Tasks creating abstractions when inline code would be clearer
  - Helper utilities or shared modules for one-time operations

  For each finding, return:
  - Task number(s) and name(s)
  - What's overcomplicated and why
  - Simpler alternative
```

#### Scope Reviewer
```
Task(subagent_type: "general-purpose", description: "Scope review of plan")
prompt: Review this implementation plan for scope creep beyond the spec.

  [PASTE FULL PLAN HERE]

  [PASTE REFERENCED SPEC IF ANY]

  Compare the plan against the spec (or stated requirements). Flag:
  - Tasks that go beyond what was specified
  - Error handling for scenarios that can't reasonably happen
  - Edge cases that are extremely unlikely and not in the spec
  - Multiple implementation options where the spec implies one
  - Testing tasks that test framework behavior rather than business logic
  - Documentation tasks beyond what's needed

  For each finding, return:
  - Task number and name
  - What exceeds scope and what was actually requested
  - Recommendation (remove, reduce, or defer)
```

#### Ordering & Dependencies Reviewer
```
Task(subagent_type: "general-purpose", description: "Dependencies review of plan")
prompt: Review this implementation plan for dependency and ordering issues.

  [PASTE FULL PLAN HERE]

  Look for:
  - Incorrect dependency ordering (task depends on something not yet built)
  - Missing dependencies (task assumes something exists that no prior task creates)
  - Unnecessary sequential dependencies (tasks marked dependent that could be parallel)
  - Tasks that should be merged because they always change together
  - Setup tasks that are heavier than needed

  For each finding, return:
  - Task number(s)
  - The ordering/dependency issue
  - Suggested fix
```

### Step 3: Synthesize Findings

Collect all reviewer results and produce a unified review:

```markdown
## Plan Review: [Plan Name]

### Summary
[1-2 sentence overall assessment: is this plan right-sized, slightly over-built, or significantly overengineered?]

### Recommendations

#### 1. [Short title]
**Type:** YAGNI / Complexity / Scope / Ordering
**Tasks affected:** #N, #M
**Issue:** [What's wrong]
**Recommendation:** [Specific change — remove task, merge tasks, simplify approach]
**Impact:** [What gets simpler if adopted]

#### 2. ...

### Tasks That Look Good
[List tasks that all reviewers agreed are well-scoped and necessary]
```

Deduplicate across reviewers — if multiple reviewers flag the same task, combine into one recommendation with the strongest reasoning.

Order recommendations by impact (biggest simplification first).

### Step 4: Interactive Resolution

Present the review to the user, then for EACH recommendation, use AskUserQuestion:

- "[Recommendation title]: [one-line summary]" (header: "Rec N")
  - "Accept — update the plan" — Apply this simplification
  - "Skip — keep as is" — The current plan is fine here
  - "Discuss — I have context" — Let the user explain why it's needed

If the user chooses "Discuss", listen to their reasoning. If it justifies the complexity, skip the recommendation. If not, suggest a compromise.

### Step 5: Update the Plan

If any recommendations were accepted:
1. Read the current plan file
2. Apply all accepted changes (remove tasks, merge tasks, simplify descriptions, fix ordering)
3. Write the updated plan back to the same file
4. Add a revision note at the top: `**Revised:** YYYY-MM-DD after plan review (N simplifications applied)`

If no recommendations were accepted, no changes needed.

### Step 6: Handoff

Use AskUserQuestion:
- "Plan review complete. What's next?" (header: "Next step")
  - "Start building (`/engineer-work`)" — Execute the plan
  - "Revise the plan further (`/engineer-plan`)" — Major rework needed
  - "Done for now" — Save and close

## Output

- Review findings presented inline
- Updated plan file (if changes accepted) at original path in `docs/plans/`

## Next Steps

- Ready to build? → `/engineer-work`
- Need a full rewrite? → `/engineer-plan`
- Want to review code after building? → `/engineer-review`
