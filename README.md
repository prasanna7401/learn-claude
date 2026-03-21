# learn-claude

A hands-on learning repository for [Claude Code](https://claude.ai/code) features. Each directory covers a distinct concept with working examples and explanations.

## Modules

| Module | What you'll learn |
|--------|-------------------|
| [`claude_code_basics/`](./claude_code_basics/) | Core concepts |
| [`hooks/`](./hooks/) | Automating tool call behavior |
| [`skills/`](./skills/) | Reusable on-demand instruction sets |
| [`sub_agents/`](./sub_agents/) | Isolated specialized Claude instances |

---

### [`claude_code_basics/`](./claude_code_basics/)

Core Claude Code concepts:

- `CLAUDE.md` file scopes and structure
- Built-in and custom slash commands
- Keyboard shortcuts
- Planning and thinking modes
- MCP server integration
- Persistent memory

**Practice project:** UIGen — a full Next.js app (AI-powered React component generator) built with Claude Code.

---

### [`hooks/`](./hooks/)

Claude Code Hooks — running custom scripts before (`PreToolUse`) or after (`PostToolUse`) any tool call.

- Blocking tool calls
- Logging and auto-formatting
- Configuration via `settings.json`
- Security best practices

**Practice project:** TypeScript e-commerce database.

---

### [`skills/`](./skills/)

Claude Code Skills — reusable `SKILL.md` instruction sets loaded on demand for specific tasks (e.g., PR review, commit formatting, doc guidelines).

---

### [`sub_agents/`](./sub_agents/)

Claude Code Sub-agents — isolated Claude instances for parallelizing work or encapsulating specialized workflows (e.g., testing, deployment, code review).
