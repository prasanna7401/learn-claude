# UIGen — Claude Code Basics

AI-powered React component generator with live preview, built to demonstrate Claude Code fundamentals.

---

## Prerequisites

- Node.js 18+
- npm

---

## Setup

1. **Optional** — Add your Anthropic API key to `.env`:

```
ANTHROPIC_API_KEY=your-api-key-here
```

> The project runs without an API key. A mock provider returns static components instead of calling Claude.

2. Install dependencies and initialize the database:

```bash
npm run setup
```

This command installs all dependencies, generates the Prisma client, and runs database migrations.

---

## Running the Application

```bash
npm run dev        # Development server (Turbopack, port 3000)
npm run build      # Production build
npm run lint       # Run ESLint
npm test           # Run tests (Vitest)
npm run db:reset   # Reset SQLite database
```

Open [http://localhost:3000](http://localhost:3000)

---

## Usage

1. Sign up or continue as an anonymous user
2. Describe the React component you want in the chat
3. View the generated component in the real-time preview
4. Switch to Code view to see and edit the generated files
5. Iterate with AI to refine your components

---

## Features

- AI-powered component generation using Claude
- Live preview with hot reload
- Virtual file system (no files written to disk)
- Syntax highlighting and Monaco code editor
- Component persistence for registered users
- Export generated code

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router), React 19, TypeScript 5 |
| Styling | Tailwind CSS v4, Shadcn/ui (Radix UI) |
| AI | Vercel AI SDK v4, `@ai-sdk/anthropic`, `claude-haiku-4-5` |
| Database | Prisma 6 + SQLite |
| Auth | JWT (Jose) + bcrypt |
| Editor | Monaco Editor |
| Testing | Vitest + @testing-library/react |

---
