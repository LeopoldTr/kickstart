<p align="center">
  <img src="https://raw.githubusercontent.com/LeopoldTr/kickstart/main/assets/logo.svg" alt="Kickstart" width="100" />
</p>

<p align="center">
  <strong>Describe it. Get a project.</strong><br/>
  AI skill that scaffolds typesafe web projects from natural language.
</p>

<p align="center">
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Agent_Skills-compatible-8B5CF6?logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48cGF0aCBkPSJNMTIgMkw0IDdWMTdMMTIgMjJMMjAgMTdWN0wxMiAyWiIgc3Ryb2tlPSJ3aGl0ZSIgc3Ryb2tlLXdpZHRoPSIyIi8+PC9zdmc+" alt="Agent Skills" /></a>
  <a href="https://www.typescriptlang.org/"><img src="https://img.shields.io/badge/TypeScript-strict-3178C6?logo=typescript&logoColor=white" alt="TypeScript" /></a>
  <a href="https://bun.sh/"><img src="https://img.shields.io/badge/Bun-runtime-f9f1e1?logo=bun&logoColor=black" alt="Bun" /></a>
  <a href="https://biomejs.dev/"><img src="https://img.shields.io/badge/Biome-lint+format-60A5FA?logo=biome&logoColor=white" alt="Biome" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License" /></a>
</p>

---

## Install

```bash
npx skills add LeopoldTr/kickstart
```

Works with **Claude Code**, **Cursor**, **Codex**, and any [Agent Skills](https://agentskills.io)-compatible agent.

### Prerequisites

This skill requires the [Context7](https://github.com/upstash/context7) MCP server to be configured in your agent. Context7 is used to fetch the latest documentation for every library before generating any file.

<details>
<summary><strong>Claude Code setup</strong></summary>

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
```

</details>

<details>
<summary><strong>Cursor / other agents</strong></summary>

Add to your MCP config:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

</details>

---

## How it works

> **Describe what you want** &rarr; **Review the proposed stack** &rarr; **Get a full project generated**

Tell your AI agent what you're building in plain English. Kickstart infers the best framework, proposes a stack for your confirmation, researches latest library versions via [Context7](https://context7.com), then generates every file directly — no `create-next-app`, no templates.

```
"I want a fullstack SaaS with user auth, a dashboard, and AI chat"

  Framework:  Next.js (App Router)
  ORM:        Drizzle + PostgreSQL
  Frontend:   Tailwind v4, shadcn/ui, zustand
  AI:         Vercel AI SDK
  Monitoring: Sentry
  CI/CD:      GitHub Actions
  Docker:     Yes
```

---

## Frameworks

| | Framework | Type | Router |
|---|---|---|---|
| <img src="https://img.shields.io/badge/-Next.js-black?logo=next.js&logoColor=white&style=flat-square" /> | **Next.js** | Fullstack | App Router (React 19) |
| <img src="https://img.shields.io/badge/-NestJS-E0234E?logo=nestjs&logoColor=white&style=flat-square" /> | **NestJS** | Backend API | Modules / Controllers |
| <img src="https://img.shields.io/badge/-React-61DAFB?logo=react&logoColor=black&style=flat-square" /> | **React** | Frontend SPA | react-router (Vite) |
| <img src="https://img.shields.io/badge/-Vue.js-4FC08D?logo=vue.js&logoColor=white&style=flat-square" /> | **Vue.js** | Frontend SPA | vue-router (Vite) |

---

## Stack

### Core — always included

Every project starts with a typesafe, opinionated foundation:

| | Purpose |
|---|---|
| **TypeScript** (strict) | `verbatimModuleSyntax`, no `any` |
| **zod** | Runtime validation, env validation |
| **neverthrow** | Result pattern — `ok()` / `err()` instead of throwing |
| **pino** | Structured logging |
| **biome** | Lint + format (replaces eslint + prettier) |
| **bun** | Package manager, runtime, test runner |
| **lefthook** | Pre-commit hooks |

### Opt-in — you choose

| Category | Options |
|---|---|
| **Frontend** | Tailwind CSS &middot; shadcn/ui &middot; Lottie &middot; i18n &middot; zustand / pinia |
| **ORM** | Drizzle *(recommended)* or TypeORM &middot; PostgreSQL |
| **Monitoring** | Sentry |
| **CI/CD** | GitHub Actions or GitLab CI |
| **Infrastructure** | Docker (multi-stage bun image) |
| **AI** | Vercel AI SDK + OpenAI |

---

## What gets generated

A complete, ready-to-run project. Not a starter template — a real architecture tailored to your choices.

<table>
  <tr>
    <td width="50%">

### Every project includes

- `package.json` with correct scripts
- Strict TypeScript config
- Environment validation (zod + `.env.example`)
- Error handling (`AppError`, `NotFoundError`, `ValidationError`)
- Result pattern (neverthrow)
- Structured logging (pino)
- Pre-commit hooks (lefthook + biome)
- **`CLAUDE.md`** for future AI sessions

  </td>
  <td width="50%">

### Adapts to your framework

| | Architecture |
|---|---|
| **Next.js** | `app/` &middot; `core/` &middot; `shared/` &middot; `infra/` &middot; `i18n/` &middot; `proxy.ts` |
| **NestJS** | `modules/` &middot; `common/` &middot; `config/` &middot; `database/` |
| **React** | `features/` &middot; `components/` &middot; `shared/` &middot; `lib/` |
| **Vue** | `features/` &middot; `composables/` &middot; `components/` &middot; `lib/` |

  </td>
  </tr>
</table>

### Example output

> *"A fullstack app with a database, Sentry, and GitHub Actions"* &rarr; Next.js + Drizzle

<table>
  <tr>
    <td>

**Application**

| Layer | Purpose |
|---|---|
| `app/` | Pages & layouts (App Router) |
| `components/ui/` | shadcn/ui components |
| `core/config/` | Env validation (zod) |
| `shared/errors/` | Typed error classes |
| `shared/result/` | neverthrow `ok()` / `err()` |
| `infra/logger/` | Pino + request context |
| `infra/storage/` | Drizzle + PostgreSQL |
| `i18n/` | next-intl (en/fr) |

  </td>
  <td>

**Tooling**

| File | What |
|---|---|
| `biome.json` | Lint + format config |
| `tsconfig.json` | Strict TypeScript |
| `drizzle.config.ts` | ORM migrations |
| `lefthook.yml` | Pre-commit hooks |
| `Dockerfile` | Multi-stage bun build |
| `docker-compose.yml` | App + PostgreSQL |
| `.github/workflows/ci.yml` | Lint, test, build |
| `.env.example` | All env vars documented |
| `CLAUDE.md` | AI-readable project guide |

  </td>
  </tr>
</table>

---

## Context7 — always up to date

Kickstart **never generates configs from memory**. Before writing any file, it uses [Context7](https://context7.com) to fetch the latest documentation for every library in the stack.

This means:
- Tailwind v4 CSS-based config, not v3's `tailwind.config.js`
- Biome v2 schema, not v1
- Latest Next.js conventions (`proxy.ts`, not `middleware.ts`)
- Current zod API, drizzle syntax, sentry SDK setup

---

## License

[MIT](LICENSE)
