---
description: Review changed code for quality, bugs, and security issues
argument-hint: <file-path>
allowed-tools: [Read, Glob, Grep, Bash]
---

Review code for quality issues. $ARGUMENTS may contain an optional file path.

**If a file path was provided in $ARGUMENTS**, read and review that specific file.

**If no file path was provided**, run `git diff --name-only` to find all changed files, then read each one.

For each file reviewed, analyze:
- Code quality and correctness
- Potential bugs or unhandled edge cases
- Security issues (XSS, injection, insecure data handling, etc.)
- Consistency with existing patterns in the codebase (check related files with Glob/Grep as needed)

Output a structured review with these sections:

## Issues
List any bugs, errors, or security vulnerabilities found (with file and line reference).

## Suggestions
List improvements for code quality, readability, or consistency.

## Summary
A brief overall assessment of the code quality.
