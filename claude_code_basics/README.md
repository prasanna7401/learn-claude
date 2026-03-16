# Claude Code Basics

Key tips and features for working with Claude Code.

---

## Setup

Run `/init` to analyze your codebase and auto-generate a `CLAUDE.md` file _(works best in CLI mode)_.

When Claude requests a tool, press **Enter** to allow a single operation or **Shift+Tab** to allow all pending operations.

### CLAUDE.md File Locations

| File | Scope |
|---|---|
| `CLAUDE.md` | Project-level — shared with the team, applies to the folder it lives in |
| `CLAUDE.local.md` | Personal/local — not committed to version control |
| `~/.claude/CLAUDE.md` | Global — applies to all projects on your machine |

> **Tip:** Keep `CLAUDE.md` under 200 lines — the entire file is loaded into context on every request.

You can reference other files inside `.md` files using `@<path>`.

### Instruction Hierarchy

Instructions are applied from broadest to most specific:

```
Global / Enterprise (managed-settings.json)
  └─ Personal (~/.claude/*)
       └─ Project (.claude/*)
            └─ Plug-in files
```

---

## Working with Claude

### Modes

| Mode | How to Trigger |
|---|---|
| **Planning mode** | Press **Shift+Tab** twice — Claude builds broad codebase understanding before making changes |
| **Thinking mode** | Use phrases in order of depth: `Think` → `Think More` → `Think a lot` → `Think longer` → `Ultrathink` |

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Escape` | Interrupt Claude to correct its context |
| `Escape` `Escape` | Revert to previous context |

---

## Commands

### Built-in Slash Commands

| Command | Description |
|---|---|
| `/init` | Analyze codebase and generate `CLAUDE.md` |
| `/clear` | Clear full context and start fresh |
| `/compact` | Summarize and compress context to keep things clean |

### Custom Slash Commands

1. Create a file inside `.claude/commands/` — the filename becomes the command
   (e.g., `terraform-test.md` → `/terraform-test`)
2. Add your instructions (setup steps, rules, etc.)
3. Restart Claude Code
4. Use `$ARGUMENTS` in your file to accept runtime arguments

> Commands only run when you explicitly type them.

---

## Memory & Context

Prefix a message with `#` to write persistent memory to `CLAUDE.md`:

```
# Always use TypeScript strict mode
```

---

## Extensions & Integrations

### MCP Servers

MCP Servers connect Claude to external applications like databases, browsers, and APIs.

**Example — add Playwright** (lets Claude interact with a browser):

```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Then prompt Claude with something like:
> *"Can you open this app at localhost:3000 and verify it in the browser?"*

To control which MCP tools Claude can use, add entries to `allow[]` in `.claude/settings.local.json` (e.g., `mcp__playwright`).
