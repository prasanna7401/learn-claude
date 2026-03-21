# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo Purpose

This is a **learning repository** for Claude Code features — not a single application. Each top-level directory is an independent module covering a distinct concept with working examples.

## Module Layout

| Module | Type | Description |
|--------|------|-------------|
| `claude_code_basics/` | Node.js app | Core Claude Code concepts; practice project is UIGen (Next.js 15 AI component generator) |
| `hooks/` | Node.js app | Claude Code Hooks (`PreToolUse`/`PostToolUse`); practice project is a TypeScript e-commerce database |
| `skills/` | Docs only | Reusable `SKILL.md` instruction sets — no build or test |
| `sub_agents/` | Docs only | Sub-agent patterns and workflows — no build or test |

## Dev Commands

### claude_code_basics (UIGen)

```bash
cd claude_code_basics
npm run setup   # install deps, generate Prisma client, run migrations
npm run dev     # Next.js dev server (Turbopack, port 3000)
npm run build   # production build
npm run lint    # ESLint
npm test        # Vitest
```

### hooks (e-commerce queries)

```bash
cd hooks
npm run setup   # install deps, init Claude hooks
npm run sdk     # run SDK script (tsx sdk.ts)
```

### skills / sub_agents

Documentation-only modules. No install, build, or test steps.

## Module-Level CLAUDE.md Files

Each buildable module has its own `CLAUDE.md` with architecture details, environment variables, and module-specific guidance:

- [`claude_code_basics/CLAUDE.md`](./claude_code_basics/CLAUDE.md)
- [`hooks/CLAUDE.md`](./hooks/CLAUDE.md)


## Formatting Recommendations

- Do NOT use em dashes (`—`). Prefer hypens (`-`) instead.