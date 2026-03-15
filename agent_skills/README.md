# Agent Skills

This directory contains examples and exercises for learning **Claude Code Skills** and **Sub-agents**.

---

## Skills

Skills are reusable, on-demand instruction sets for Claude. They live in `.claude/skills/<skill-name>/SKILL.md`.

### How Skills Work

- All skill names and descriptions are loaded when Claude starts — not on every message.
- The full `SKILL.md` content is only loaded when the skill is triggered.
- `CLAUDE.md` is considered in every conversation; skills are loaded on demand for specific tasks (e.g., PR review, commit formatting, doc guidelines).
- Claude relies on the description to decide when to trigger a skill. Write clear, specific descriptions.

### SKILL.md Structure

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

### File Organization

Split content across files — don't put everything in one file:

| Folder | Purpose |
|--------|---------|
| `scripts/` | Code and automation |
| `references/` | Documentation and guides |
| `assets/` | Images and media |
| `templates/` | Reusable templates |

### Troubleshooting

If a skill doesn't trigger (overlapping names, non-matching descriptions, priority conflicts), use:
- `claude --debug` to inspect skill loading
- The `agent-skills-verifier` tool

---

## Sub-agents

Sub-agents are specialized Claude instances that run with a **fresh context**, isolated from the main conversation. They live in `.claude/agents/<agent-name>.md`.

### Sub-agent Frontmatter

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

### When to Use Sub-agents

Use sub-agents to:
- Isolate long-running or noisy tasks from the main context
- Parallelize independent workstreams
- Encapsulate specialized workflows (e.g., testing, deployment, code review)

---

## Quick Reference

| Concept | Location | Loaded |
|---------|----------|--------|
| Global instructions | `CLAUDE.md` | Every conversation |
| Skill instructions | `.claude/skills/<name>/SKILL.md` | On demand |
| Sub-agent definitions | `.claude/agents/<name>.md` | When invoked |

Check available skills at any time with `/skills`.
