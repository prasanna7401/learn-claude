
## Claude Code Basics

This project was built using Claude Code. Below are the key tips and features used during development.

---

### Setup

- Run `/init` in your project to analyze the codebase and auto-generate a `CLAUDE.md` file (works best in CLI mode)
- Press **Enter** to allow a single operation, or **Shift+Tab** to allow all pending operations

#### CLAUDE.md File Locations

| File | Scope |
|---|---|
| `CLAUDE.md` | Project-level — shared with the team, applies to the folder it lives in |
| `CLAUDE.local.md` | Personal/local — not committed to version control |
| `~/.claude/CLAUDE.md` | Global — applies to all projects on your machine |

> Keep CLAUDE.md under 200 lines — the entire file is loaded into context.

You can reference other files inside `.md` files using `@<path>`.

#### Instruction Hierarchy

```
Global / Enterprise (managed-settings.json)
  └─ Personal (~/.claude/*)
       └─ Project (.claude/*)
            └─ Plug-in files
```

---

### Working with Claude

#### Modes

| Mode | How to Trigger |
|---|---|
| Planning mode | Press **Shift+Tab** twice — broad codebase understanding before making changes |
| Thinking mode | Use phrases: `Think` < `Think More` < `Think a lot` < `Think longer` < `Ultrathink` |

#### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Escape` | Interrupt Claude to correct context |
| `Escape + Escape` | Revert to previous context |

---

### Commands

#### Built-in Slash Commands

| Command | Description |
|---|---|
| `/init` | Analyze codebase and generate `CLAUDE.md` |
| `/clear` | Clear full context and start fresh |
| `/compact` | Summarize and compress context to keep the terminal clean |

#### Custom Slash Commands

1. Create a file inside `.claude/commands/` — the filename becomes the command
   (e.g., `terraform-test.md` → `/terraform-test`)
2. Add your instructions (setup steps, rules, etc.)
3. Restart Claude Code
4. Use `$ARGUMENTS` in your file to accept arguments at runtime

Commands only run when you explicitly type them.

---

### Memory & Context

Use `#` at the start of a message to set persistent memory for a project (writes to `CLAUDE.md`).

---

### Extensions & Integrations

#### MCP Servers

MCP Servers simplify integration with external applications (databases, browsers, APIs).

**Add Playwright MCP** (lets Claude interact with a browser):
```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Example prompt: *"Can you open this app at localhost:3000 and verify it in the browser?"*

Control Claude's MCP permissions in `.claude/settings.local.json` by adding entries to `allow[]`
(e.g., `mcp__playwright`).
