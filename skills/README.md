# Skills

This directory contains examples and exercises for learning **Claude Code Skills** — reusable, on-demand instruction sets that live in `.claude/skills/<skill-name>/SKILL.md`.

---

## How Skills Work

- All skill names and descriptions are loaded when Claude starts — not on every message.
- The full `SKILL.md` content is only loaded when the skill is triggered.
- `CLAUDE.md` is considered in every conversation; skills are loaded on demand for specific tasks (e.g., PR review, commit formatting, doc guidelines).
- Claude relies on the description to decide when to trigger a skill. Write clear, specific descriptions.

## SKILL.md Structure

```markdown
---
name: skill-name
description: |
  What the skill does, when it should be triggered,
  and what it accomplishes. (~2-3 lines)
allowed-tools: Bash, Read, Edit   # optional
model: claude-sonnet-4-6          # optional
---

Your skill instructions go here...
```

## File Organization

Split content across files — don't put everything in one file:

| Folder | Purpose |
|--------|---------|
| `scripts/` | Code and automation |
| `references/` | Documentation and guides |
| `assets/` | Images and media |
| `templates/` | Reusable templates |

## Troubleshooting

If a skill doesn't trigger (overlapping names, non-matching descriptions, priority conflicts), use:
- `claude --debug` to inspect skill loading
- The `agent-skills-verifier` tool

---

## Best Practices

### 1. Write `description` as a trigger condition, not a summary

Claude uses the description to decide **when** to load the skill. A vague description means it either fires too often or never fires at all.

```markdown
# Good — specific trigger
description: |
  Use when creating or updating pull requests. Enforces
  conventional commit messages, PR title format, and test plan checklist.

# Bad — too vague
description: |
  Helps with pull requests.
```

### 2. Split supporting content into separate files

`SKILL.md` should contain **instructions**, not reference data. Put checklists, templates, examples, and scripts in sibling files and reference them from the skill.

```
.claude/skills/pr-review/
├── SKILL.md              # Instructions only (~300-500 words)
├── references/
│   └── checklist.md      # Detailed review checklist
├── templates/
│   └── pr-body.md        # PR description template
└── scripts/
    └── lint-commits.sh   # Automation helper
```

This keeps the skill focused and avoids bloating the context when it loads.

### 3. Use `allowed-tools` to limit scope

Like sub-agents, skills benefit from tight tool scoping. A documentation skill doesn't need `Bash`; a linting skill doesn't need `WebSearch`.

```markdown
# Documentation skill — read-only is enough
allowed-tools: Read, Glob, Grep

# Code generation skill — needs write access
allowed-tools: Read, Edit, Glob, Grep, Bash
```

### 4. Avoid overlapping skill descriptions

When two skills have similar descriptions, Claude may pick the wrong one — or neither. Each skill should own a distinct trigger surface.

```markdown
# Bad — both skills compete for the same trigger
skill-a description: "Use when writing tests"
skill-b description: "Use when testing code"

# Good — clear boundaries
skill-a description: "Use when writing unit tests with Jest for frontend components"
skill-b description: "Use when running E2E tests with Playwright for integration flows"
```

If you notice a skill not triggering, check for overlaps with `claude --debug`.

### 5. Keep instructions under ~500 words

Skills load into the active context on demand. Long instructions compete with the user's actual task for attention. Be precise and concise — reference external files for details rather than inlining everything.

### Skills vs Sub-agents: when to use which

| Use a **Skill** when... | Use a **Sub-agent** when... |
|---|---|
| You need reusable instructions injected into the current conversation | The task should run in an isolated context |
| The task is part of the main workflow (e.g., formatting, conventions) | The task is independent and could run in parallel |
| Output should flow directly into the conversation | The task is long-running or produces noisy intermediate output |
| You want Claude to follow specific patterns while working | You want a specialized persona with its own tool set |

---

## Quick Reference

| Concept | Location | Loaded |
|---------|----------|--------|
| Global instructions | `CLAUDE.md` | Every conversation |
| Skill instructions | `.claude/skills/<name>/SKILL.md` | On demand |

Check available skills at any time with `/skills`.
