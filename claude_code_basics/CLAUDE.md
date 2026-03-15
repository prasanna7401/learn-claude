# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**UIGen** is an AI-powered React component generator with a live preview interface. Users describe React components in natural language and Claude generates them in real-time. It is a full-stack Next.js 15 app with authentication, project persistence, and a virtual file system.

## Commands

```bash
# Initial setup (install deps, generate Prisma client, run migrations)
npm run setup

# Development server (Turbopack, port 3000)
npm run dev

# Production build
npm run build

# Lint
npm run lint

# Run tests
npm test

# Reset SQLite database
npm run db:reset
```

## Architecture

### Request Flow

```
Client (React) → Context API → Server Actions / API Routes → Prisma → SQLite
                                      ↓
                              AI Streaming (Vercel AI SDK + Claude)
```

### Key Subsystems

**Virtual File System** (`src/lib/file-system.ts`)
In-memory file tree managed by `VirtualFileSystem` class. No disk I/O — state is serialized to JSON and stored in the `Project.data` database column. Loaded and deserialized on project open.

**AI Integration** (`src/app/api/chat/`, `src/lib/tools/`, `src/lib/provider.ts`)
Uses Vercel AI SDK with `@ai-sdk/anthropic`. Claude (`claude-haiku-4-5`) responds via streaming and calls two tools:
- `str_replace_editor` — edits file contents
- `file_manager` — creates/deletes files and directories

Falls back to `MockLanguageModel` when `ANTHROPIC_API_KEY` is not set, returning static sample components.

**Server Actions** (`src/actions/`)
"use server" functions handle auth (`signUp`, `signIn`, `signOut`, `getUser`) and project CRUD. JWT sessions stored in HTTP-only cookies; secret defaults to `"development-secret-key"` if `JWT_SECRET` is unset.

**State Management** (`src/lib/contexts/`)
Two React contexts:
- `FileSystemProvider` — virtual file tree state
- `ChatProvider` — chat messages and AI streaming state

**Preview** (`src/components/preview/`, `src/lib/transform/`)
Babel Standalone transpiles JSX client-side; components render inside an iframe for isolation with hot reload.

### Database Schema

```
User (id, email, password, createdAt, updatedAt)
  └─ Project (id, name, userId?, messages: JSON, data: JSON, createdAt, updatedAt)
```

## Environment Variables

| Variable | Required | Notes |
|---|---|---|
| `ANTHROPIC_API_KEY` | No | Falls back to mock provider if absent |
| `JWT_SECRET` | No | Defaults to `"development-secret-key"` |

## Tech Stack

- **Framework:** Next.js 15 (App Router), React 19, TypeScript 5
- **Styling:** Tailwind CSS v4, Shadcn/ui (New York style, Radix UI primitives)
- **AI:** Vercel AI SDK v4, `@ai-sdk/anthropic`, model `claude-haiku-4-5`
- **Database:** Prisma 6 + SQLite (`prisma/dev.db`)
- **Auth:** JWT via Jose, bcrypt for password hashing
- **Editor:** Monaco Editor
- **Testing:** Vitest + @testing-library/react (jsdom environment)
- **Path alias:** `@/*` → `src/*`
