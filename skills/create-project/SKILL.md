---
name: create-project
description: Scaffold a new typesafe web project with modern tooling. Use when the user wants to create, bootstrap, or scaffold a new project, app, or boilerplate — whether fullstack, frontend SPA, or backend API. Supports Next.js, NestJS, React (Vite), and Vue.js with opinionated defaults for type safety, error handling, and DX.
---

# Create Project

Interactively scaffold a typesafe project adapted to the user's needs. The user describes what they want to build in natural language. You infer the best stack, propose it for confirmation, then generate all files directly.

## Workflow

1. **Understand** — Ask the user what they're building (purpose, audience, features needed)
2. **Propose** — Recommend a framework + libs, present as a table, get confirmation
3. **Research** — Use context7 (`resolve-library-id` then `query-docs`) for EVERY lib to get latest versions, config formats, and integration patterns. Never generate configs from memory.
4. **Generate** — Write all project files directly (no `create-next-app` or similar)
5. **Install & verify** — Run `bun install` and `bunx tsc --noEmit` to confirm everything compiles
6. **CLAUDE.md** — Generate a comprehensive CLAUDE.md for the new project

## Framework Selection

| Signal | Framework |
|--------|-----------|
| Fullstack, SSR, server components, SEO | **Next.js** (App Router) |
| Backend API, microservices, REST/GraphQL | **NestJS** |
| Frontend SPA, client-side app, dashboard | **React** (Vite) |
| Vue preference, progressive framework | **Vue.js** (Vite) |

## Lib Categories

### Core (always included)

Every project gets these — non-negotiable for type safety and DX:

| Lib | Purpose |
|-----|---------|
| TypeScript (strict) | Type safety, `verbatimModuleSyntax` |
| zod | Runtime validation, env validation, schema definitions |
| neverthrow | Result pattern — `ok()` / `err()` instead of throwing |
| pino + pino-pretty | Structured logging (pino-pretty as devDependency) |
| biome | Linting + formatting (replaces eslint + prettier) |
| bun | Package manager + runtime + test runner |
| lefthook | Pre-commit hooks running `bun run lint` |

### Frontend (Next.js, React, Vue)

Next.js uses React — all React libs (shadcn/ui, zustand, lottie-react) apply to both Next.js and React Vite projects.

| Lib | Scope | Notes |
|-----|-------|-------|
| tailwindcss | All frontend | CSS framework |
| shadcn/ui | Next.js + React | Component library (new-york style, slate base) |
| lottie-react | Next.js + React | Lottie animations for rich UI (loaders, illustrations) |
| vue3-lottie | Vue only | Lottie animations for Vue |
| zustand | Next.js + React | State management |
| pinia | Vue only | State management |
| i18n | All frontend | next-intl (Next.js) / react-i18next (React) / vue-i18n (Vue) |

When proposing the stack, mention LottieFiles as an option for projects that benefit from animations (landing pages, dashboards, onboarding flows).

### Backend / Database (opt-in)

Ask the user if they need a database. If yes, ask which ORM they prefer:

| Option | Libs | Style |
|--------|------|-------|
| **Drizzle** (recommended) | drizzle-orm + drizzle-kit + postgres | Lightweight, SQL-like, schema-as-code |
| **TypeORM** | typeorm + pg | Decorator-based, Active Record or Data Mapper |

Both use PostgreSQL. Drizzle is recommended for new projects (lighter, better TypeScript inference). TypeORM is a good choice if the user prefers decorator patterns or comes from a NestJS/Java background.

### Observability (opt-in)

Ask the user if they want error monitoring. If yes:

| Lib | Purpose |
|-----|---------|
| @sentry/nextjs / @sentry/nestjs / @sentry/react / @sentry/vue | Error tracking, performance monitoring, session replay |

Sentry integrates differently per framework — always use context7 to get the latest SDK setup for the chosen framework. Includes source maps upload, error boundaries, and server-side error capture.

### CI/CD (opt-in)

Ask the user which CI/CD platform they use:

| Option | File | Notes |
|--------|------|-------|
| **GitHub Actions** | `.github/workflows/ci.yml` | Lint, type-check, test, build on push/PR |
| **GitLab CI** | `.gitlab-ci.yml` | Same stages, GitLab runners |
| **None** | — | Skip CI setup |

### Docker (opt-in)

Ask the user if they need Docker. If yes, generate:

| File | Purpose |
|------|---------|
| `Dockerfile` | Multi-stage build (bun install → build → production image) |
| `docker-compose.yml` | App + PostgreSQL (if DB selected) + any other services |
| `.dockerignore` | Exclude node_modules, .git, etc. |

Use `oven/bun` as base image. Multi-stage build: deps → build → slim production image.

### AI (opt-in)

Ask the user if they need AI features. If yes:

| Lib | Purpose |
|-----|---------|
| ai (Vercel AI SDK) | `generateText`, `streamText`, `generateObject` |
| @ai-sdk/openai | OpenAI provider |

## Context7 Research (CRITICAL)

**Before generating ANY config or code file, you MUST use context7 to verify current APIs.**

For each lib in the chosen stack:

1. Call `resolve-library-id` with the lib name
2. Call `query-docs` to fetch current installation, config format, and usage patterns
3. Use the fetched docs as source of truth — never rely on training data for configs

**Research order:**
1. Framework (Next.js / NestJS / Vite / Vue)
2. TypeScript config for the framework
3. Core libs (zod, biome, pino)
4. Feature libs (tailwind, drizzle or typeorm, lottie, sentry, AI SDK)
5. Integration libs (shadcn/ui setup, next-intl, etc.)

**Pay special attention to:**
- Config file format changes (tailwind v4 vs v3, biome v2, zod v4)
- Framework-specific integration (how next-intl works with latest Next.js)
- Breaking changes between major versions

## File Generation

Generate all files directly. Load only the references relevant to the user's chosen stack:

### Frameworks (load ONE based on user choice)
- [Next.js](references/frameworks/nextjs.md) — App Router, proxy.ts, server components
- [NestJS](references/frameworks/nestjs.md) — Modules/services/controllers
- [React (Vite)](references/frameworks/react.md) — SPA with react-router
- [Vue.js](references/frameworks/vuejs.md) — Composition API, vue-router

### Frontend (load if frontend framework selected)
- [Tailwind](references/frontend/tailwind.md) — v4 CSS-based config, per-framework plugin
- [shadcn/ui](references/frontend/shadcn.md) — components.json, cn() utility (React only)
- [Lottie](references/frontend/lottie.md) — Animation setup per framework
- [i18n](references/frontend/i18n.md) — next-intl / react-i18next / vue-i18n

### ORM (load ONE if DB selected)
- [Drizzle](references/orm/drizzle.md) — Config, connection singleton, scripts
- [TypeORM](references/orm/typeorm.md) — DataSource, entities, scripts

### Monitoring (load if selected)
- [Sentry](references/monitoring/sentry.md) — SDK per framework, config files

### CI/CD (load ONE if selected)
- [GitHub Actions](references/cicd/github-actions.md) — Workflow config
- [GitLab CI](references/cicd/gitlab-ci.md) — Pipeline config

### Infrastructure
- [Docker](references/infra/docker.md) — Dockerfile, compose, dockerignore
- [Core patterns](references/infra/core-patterns.md) — biome, tsconfig, lefthook, env validation, errors, result, logger

## CLAUDE.md Generation

Every project gets a CLAUDE.md with:

1. **Commands** — dev, build, lint, test, db commands as a table
2. **Architecture** — Layer table with purpose for each directory
3. **Key patterns** — Result pattern, env validation, logging, error classes
4. **Code style** — Biome config, TypeScript strictness, conventions
5. **Critical rules** — Required/forbidden patterns

Use the project's actual structure and choices — don't use a generic template.

## Common Mistakes

- Generating tailwind v3 config when v4 uses CSS-based config
- Using `middleware.ts` instead of `proxy.ts` for Next.js 16+
- Forgetting `verbatimModuleSyntax` in tsconfig
- Not wrapping postgres driver with global singleton for dev HMR
- Using eslint/prettier instead of biome
- Forgetting to add `pino-pretty` as devDependency only
