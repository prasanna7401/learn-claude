# Sub-agents

This directory contains examples and exercises for learning **Claude Code Sub-agents** — specialized Claude instances that run with a fresh context, isolated from the main conversation. They live in `.claude/agents/<agent-name>.md`.

---

## Sub-agent Frontmatter

```markdown
---
name: agent-name
description: When this agent should be invoked and what it does.
tools: Bash, Read, Edit, Glob
model: claude-sonnet-4-6
color: blue
skills:
  - skill-name
---

Agent instructions go here...
```

## When to Use Sub-agents

Use sub-agents to:
- Isolate long-running or noisy tasks from the main context
- Parallelize independent workstreams
- Better for research tasks than chaining operations across multiple agents (unless you use 'Agent Teams')
- Reduce the chance of hitting your context window in the main/parent agent.
- Encapsulate specialized workflows (e.g., testing, deployment, code review)

## Make Claude use the Sub-agent automatically

- Include the word **proactively** in the frontmatter description, so that you don't have to explicitly ask Claude to run it. For example:
`description: Proactively suggest running this agent when...`

## How to define a good Sub-agent?

- **PROMPT** Strategy - not all letters carry equal weight:

| Letter | Meaning | Impact | Notes |
|--------|---------|--------|-------|
| **P** | Purpose | High | The single most important element. Clearly stating the task drives behavior more than anything else. |
| **R** | Role | Low | Generic labels like "senior engineer" add almost nothing - Claude already has this knowledge. Only useful if you define *behavioral* guidance (e.g., "cite file paths and line numbers") instead of a title. |
| **O** | Output format / Obstacle Reporting | High | Defining exact output structure dramatically reduces token waste and improves consistency. |
| **M** | Markers / Guardrails / Scope | High | Negative constraints ("Do NOT modify files") and scope boundaries are highly effective. |
| **T** | Tone | Low | Minor effect. Worth one line at most, not a full section. |

- While **PROMPT** works well for single-task agents, multi-step agents benefit from the **TRACE** pattern (every letter maps to something behaviorally concrete):

| Letter | Meaning | Example |
|--------|---------|---------|
| **T** | Task | "Run the full test suite and categorize failures" |
| **R** | Resources | "Read `jest.config.ts` and all `*.test.ts` files" |
| **A** | Actions | "1. Run tests 2. Parse output 3. Classify failures" |
| **C** | Criteria | "Success = all tests pass or failures are categorized with root causes" |
| **E** | Escalation | "If >10 tests fail, stop and report instead of fixing" |

Use **PROMPT** for focused, single-purpose agents. Use **TRACE** when the agent must plan, execute, and evaluate across multiple steps.

For example: When creating a `code-reviewer` agent, by defining a clear output format, you reduce the token usage and time consumed.

```markdown
---
name: code-quality-reviewer
description: Proactively review code for quality, security, and best practices when code is written or modified.
tools: Read, Glob, Grep
model: claude-sonnet-4-6
color: green
---

# Purpose
You are a senior code reviewer. Analyze the changed files for code quality, security vulnerabilities, and adherence to best practices.

# Role
Act as a senior engineer performing a thorough code review. Be constructive and specific — cite file paths and line numbers.

# Output Format
Structure your review using EXACTLY these sections:

## Summary
One-paragraph overview of what was changed and overall quality assessment.

## Critical Issues
Security vulnerabilities, data loss risks, or breaking changes that MUST be fixed before merge.

## Issues
- `Major`: Bugs, performance problems, or significant design concerns that SHOULD be fixed.
- `Minor`: Style inconsistencies, naming suggestions, or small improvements that COULD be fixed.

## Recommendations
Broader architectural or pattern suggestions for future consideration.

## Approval Status
One of: APPROVED, APPROVED WITH SUGGESTIONS, CHANGES REQUESTED, BLOCKED

## Obstacles Encountered
Report any obstacles encountered during the review process. This can be: setup issues, workarounds discovered or environment quirks. Report commands that needed a special flag or configuration. Report dependencies or imports that caused problems.

# Markers / Guardrails
- Only review files that have been changed (use git diff to identify them)
- Do NOT apply fixes automatically — report findings only
- Flag any hardcoded secrets, SQL injection risks, or XSS vulnerabilities as Critical
- Limit scope to the current changeset; do not review unrelated code

# Tone
Professional and constructive. Explain WHY something is an issue, not just that it is one.
```

> The **Obstacles Encountered** section is very useful, as it helps in identifying areas of improvement for your agent.

---

## Best Practices

### 1. Define a rigid output format

This is the single highest-impact practice. When the agent knows exactly what shape to return, it spends fewer tokens wandering and produces results you can parse programmatically.

```markdown
# Output Format
Return ONLY a JSON object with this schema:
{ "status": "pass | fail", "issues": [{ "file": "", "line": 0, "severity": "", "message": "" }] }
```

### 2. Use negative constraints

Telling the agent what **not** to do is often more effective than positive-only instructions. Models follow "Do NOT..." directives more reliably than implied boundaries.

```markdown
- Do NOT modify any files — report findings only
- Do NOT review files outside the current changeset
- Do NOT suggest style changes for code you didn't analyze
```

### 3. Scope tools tightly

Only grant the tools the agent actually needs. Fewer tools means fewer ways to go off-script — this is a mechanical guardrail, not just a suggestion.

| Task | Recommended Tools |
|------|-------------------|
| Code review | `Read, Glob, Grep` |
| Refactoring | `Read, Edit, Glob, Grep` |
| Research | `Read, Glob, Grep, WebSearch, WebFetch` |
| Build fixing | `Bash, Read, Edit, Glob, Grep` |

### 4. Write `description` as a trigger condition

The `description` field is used for **routing**, not documentation. Write it as "when X happens, invoke this agent" so Claude knows when to reach for it automatically.

```markdown
# Good — acts as a trigger
description: Proactively run when code is modified to review for quality and security issues.

# Bad — just describes what it is
description: A code review agent.
```

### 5. Keep instructions under ~500 words

Sub-agents start with a fresh context window. Long instructions dilute the model's attention before it even begins the task. Aim for **300–500 words** — enough to be precise, short enough to stay focused.

---

## Quick Reference

| Concept | Location | Loaded |
|---------|----------|--------|
| Global instructions | `CLAUDE.md` | Every conversation |
| Sub-agent definitions | `.claude/agents/<name>.md` | When invoked |
