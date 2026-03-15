# Claude Code Hooks

Hooks let you run custom scripts **before or after** any Claude tool call, giving you control over Claude's behavior without modifying your codebase.

## How Hooks Work

| Hook Type | When It Runs | Can Block Claude? |
|-----------|-------------|-------------------|
| `PreToolUse` | Before a tool call executes | Yes — return a non-zero exit code |
| `PostToolUse` | After a tool call completes | No — but can send feedback to Claude |

## Common Use Cases

- **Block access to sensitive files** — e.g., prevent Claude from reading `.env` files
- **Auto-format code** — run `terraform fmt` after a `.tf` file is edited
- **Run tests** — trigger your test suite after code changes
- **Lint on save** — enforce code style after every file write
- **Propagate changes** — ensure a change in one file is reflected in related files
- **Verify with Claude** — spawn a second Claude Code session to review changes (watch for loops)

## Configuration

Hooks are defined in JSON settings files:

| Scope | File Location |
|-------|--------------|
| Repository | `.claude/settings.json` |
| Global | `~/.claude/settings.json` |

You can write hooks by hand or use the `/hooks` command inside Claude Code. Restart Claude after making changes.

## Example: Block Reads on `.env` Files (PreToolUse)

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Read",
        "command": "/absolute/path/to/block-env-read.sh"
      }
    ]
  }
}
```

The script should exit with code `2` to block the tool call, or `0` to allow it.

## Example: Auto-format Terraform After Edit (PostToolUse)

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "command": "/absolute/path/to/run-tf-fmt.sh"
      }
    ]
  }
}
```

## Tips & Best Practices

- **Use absolute paths** — relative paths can be exploited if Claude is tricked into running a malicious script. Prefer `$PWD/<path>` for convenience.
- **Keep hooks lightweight** — use the Claude TypeScript SDK where possible to minimize overhead.
- **Hooks work with MCPs** — you can monitor tool calls from any MCP server, not just built-in tools.
- **Debug with a wildcard matcher** — use `"matcher": "*"` and pipe output to a log file (e.g., `jq . > post-log.json`) to inspect what data hooks receive.
- **Avoid infinite loops** — if spawning a nested Claude session inside a hook, add guards to prevent recursive triggering.

## Hook Coverage

Hooks can monitor any tool call available to Claude — built-in tools (`Read`, `Grep`, `Edit`, `Bash`, etc.) as well as tools added via MCP servers. There are approximately 20 hook event types available.

## Security Considerations

Hooks run as shell commands with the permissions of the current user. Always:

- Validate inputs before acting on them
- Use absolute paths to scripts
- Be cautious when hooks trigger external processes or network calls
