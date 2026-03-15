# learn-claude

A hands-on learning repository for [Claude Code](https://claude.ai/code) features. Each directory covers a distinct concept with working examples and explanations.

## Directories

### [`claude_code_basics/`](./claude_code_basics/)
- Core Claude Code concepts: `CLAUDE.md` file scopes and structure, built-in and custom slash commands, keyboard shortcuts, planning and thinking modes, MCP server integration, and persistent memory.
- Includes a full Next.js app (UIGen — an AI-powered React component generator) built with Claude Code as a practical reference.

### [`hooks/`](./hooks/)
- Claude Code Hooks — running custom scripts before (`PreToolUse`) or after (`PostToolUse`) any tool call.
- Covers blocking tool calls, logging, auto-formatting, configuration via `settings.json`, and security best practices. 
- Uses a TypeScript e-commerce database project as the practice codebase.

### [`agent_skills/`](./agent_skills/)
- Claude Code Skills and Sub-agents. Skills are reusable `SKILL.md` instruction sets loaded on demand.
- Sub-agents are isolated Claude instances used to parallelize work or encapsulate specialized workflows like testing or code review.
