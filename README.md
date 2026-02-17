# Kickstart

Opinionated project scaffolding skill for AI agents. Interactively creates typesafe web projects with modern tooling.

## Install

```bash
npx skills add leopoldtr/kickstart
```

## What it does

Describe what you want to build in natural language. The skill infers the best stack, proposes it for confirmation, researches latest lib versions via [Context7](https://context7.com), then generates all project files directly.

## Supported Frameworks

| Framework | Type |
|-----------|------|
| **Next.js** | Fullstack (App Router, React, SSR) |
| **NestJS** | Backend API |
| **React** (Vite) | Frontend SPA |
| **Vue.js** (Vite) | Frontend SPA |

## Stack

### Core (always included)
TypeScript (strict) · zod · neverthrow · pino · biome · bun · lefthook

### Opt-in
- **Frontend**: Tailwind CSS · shadcn/ui · Lottie · i18n · zustand/pinia
- **ORM**: Drizzle (recommended) or TypeORM · PostgreSQL
- **Monitoring**: Sentry
- **CI/CD**: GitHub Actions or GitLab CI
- **Infrastructure**: Docker (multi-stage bun image)
- **AI**: Vercel AI SDK

## Usage

Once installed, just tell your AI agent what you want to build:

> "I want to build a fullstack app with user auth, a dashboard, and AI chat"

The skill will:
1. Infer the best framework and libs
2. Propose the stack for your confirmation
3. Research latest versions via Context7
4. Generate all files (no `create-next-app`)
5. Run `bun install` and verify compilation
6. Generate a `CLAUDE.md` for future sessions

## License

MIT
