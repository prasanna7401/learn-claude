---
description: Run ESLint, auto-fix issues, and report what remains
allowed-tools: [Bash]
---

Run ESLint and auto-fix issues:

1. Run `npm run lint` to surface all current ESLint errors and warnings.
2. Run `npm run lint -- --fix` to automatically fix any fixable issues.
3. Run `npm run lint` again to check what remains after the fixes.

Report:
- What was automatically fixed
- What issues still require manual attention (with file and line references)
- An overall lint health summary
