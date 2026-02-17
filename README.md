<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/LeopoldTr/kickstart/main/assets/banner-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/LeopoldTr/kickstart/main/assets/banner-light.svg">
    <img src="https://raw.githubusercontent.com/LeopoldTr/kickstart/main/assets/banner-dark.svg" alt="Kickstart" width="720" />
  </picture>
</p>

<p align="center">
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Agent_Skills-compatible-8B5CF6?style=for-the-badge" alt="Agent Skills" /></a>
  &nbsp;
  <a href="https://context7.com"><img src="https://img.shields.io/badge/Context7-powered-0EA5E9?style=for-the-badge" alt="Context7" /></a>
  &nbsp;
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License" /></a>
</p>

<br/>

<p align="center">
  <code>npx skills add LeopoldTr/kickstart</code>
</p>

<br/>

<p align="center">
  <img src="https://raw.githubusercontent.com/LeopoldTr/kickstart/main/assets/terminal.svg" alt="Kickstart in action" width="720" />
</p>

<br/>

## What is this?

An [Agent Skill](https://agentskills.io) that turns a sentence into a production-ready project. Tell your AI agent what you're building — it picks the right framework, proposes a stack, looks up the **latest docs** for every library, and generates all files from scratch.

No templates. No `create-next-app`. Every config verified against current documentation via [Context7](https://context7.com).

Works with **Claude Code** &middot; **Cursor** &middot; **Codex** &middot; any skills-compatible agent.

---

## Frameworks

<table>
  <tr>
    <td align="center" width="25%">
      <img src="https://img.shields.io/badge/-black?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js" /><br/>
      <strong>Next.js</strong><br/>
      <sub>Fullstack &middot; App Router &middot; React 19</sub>
    </td>
    <td align="center" width="25%">
      <img src="https://img.shields.io/badge/-E0234E?style=for-the-badge&logo=nestjs&logoColor=white" alt="NestJS" /><br/>
      <strong>NestJS</strong><br/>
      <sub>Backend API &middot; Modules &middot; Controllers</sub>
    </td>
    <td align="center" width="25%">
      <img src="https://img.shields.io/badge/-61DAFB?style=for-the-badge&logo=react&logoColor=black" alt="React" /><br/>
      <strong>React</strong><br/>
      <sub>Frontend SPA &middot; Vite &middot; react-router</sub>
    </td>
    <td align="center" width="25%">
      <img src="https://img.shields.io/badge/-4FC08D?style=for-the-badge&logo=vue.js&logoColor=white" alt="Vue" /><br/>
      <strong>Vue.js</strong><br/>
      <sub>Frontend SPA &middot; Vite &middot; vue-router</sub>
    </td>
  </tr>
</table>

---

## Stack

<table>
  <tr>
    <td width="50%">

### Core — every project

|   | |
|---|---|
| <img src="https://img.shields.io/badge/-3178C6?style=flat-square&logo=typescript&logoColor=white" /> | **TypeScript** strict, `verbatimModuleSyntax` |
| <img src="https://img.shields.io/badge/-3E67B1?style=flat-square&logo=zod&logoColor=white" /> | **zod** validation, env schemas |
| | **neverthrow** `ok()` / `err()` result pattern |
| <img src="https://img.shields.io/badge/-60A5FA?style=flat-square&logo=biome&logoColor=white" /> | **biome** lint + format |
| <img src="https://img.shields.io/badge/-f9f1e1?style=flat-square&logo=bun&logoColor=black" /> | **bun** runtime, package manager, test runner |
| | **pino** structured logging |
| | **lefthook** pre-commit hooks |

  </td>
  <td width="50%">

### Opt-in — you choose

|   | |
|---|---|
| <img src="https://img.shields.io/badge/-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" /> | **Tailwind CSS** v4 |
| <img src="https://img.shields.io/badge/-000?style=flat-square&logo=shadcnui&logoColor=white" /> | **shadcn/ui** components |
| | **Drizzle** or **TypeORM** + PostgreSQL |
| | **Sentry** error monitoring |
| | **Vercel AI SDK** + OpenAI |
| | **Lottie** animations |
| | **i18n** (next-intl / react-i18next / vue-i18n) |
| <img src="https://img.shields.io/badge/-2088FF?style=flat-square&logo=githubactions&logoColor=white" /> | **GitHub Actions** or **GitLab CI** |
| <img src="https://img.shields.io/badge/-2496ED?style=flat-square&logo=docker&logoColor=white" /> | **Docker** multi-stage bun image |

  </td>
  </tr>
</table>

---

## What gets generated

<table>
  <tr>
    <td width="50%">

### Architecture (adapted per framework)

| | Next.js | NestJS | React / Vue |
|---|---|---|---|
| **App layer** | `app/` | `modules/` | `features/` |
| **Components** | `components/ui/` | — | `components/ui/` |
| **Domain** | `core/` | `modules/*/` | `features/*/` |
| **Shared** | `shared/` | `common/` | `shared/` |
| **Infra** | `infra/` | `database/` | `lib/` |
| **i18n** | `i18n/` | — | `locales/` |

  </td>
  <td width="50%">

### Tooling (always generated)

| File | |
|---|---|
| `package.json` | Scripts for dev, build, lint, test, db |
| `tsconfig.json` | Strict TypeScript |
| `biome.json` | Lint + format rules |
| `lefthook.yml` | Pre-commit hooks |
| `.env.example` | All env vars documented |
| `CLAUDE.md` | AI-readable project guide |
| `Dockerfile` | Multi-stage bun build *(opt-in)* |
| `ci.yml` | Lint, test, build pipeline *(opt-in)* |

  </td>
  </tr>
</table>

### Patterns in every project

```typescript
// Result pattern — no more try/catch
import { ok, err, type Result } from "@/shared/result";

function findUser(id: string): Result<User, NotFoundError> {
  const user = db.query.users.findFirst({ where: eq(users.id, id) });
  return user ? ok(user) : err(new NotFoundError("User", id));
}
```

```typescript
// Env validation — fail fast at startup
const env = z.object({
  DATABASE_URL: z.string().min(1),
  OPENAI_API_KEY: z.string().min(1),
}).parse(process.env);
```

---

## Prerequisites

This skill uses [Context7](https://context7.com) to look up the latest docs for every library. Make sure it's configured:

<details>
  <summary>&nbsp;<strong>Claude Code</strong></summary>

  ```bash
  claude mcp add context7 -- npx -y @upstash/context7-mcp@latest
  ```

</details>

<details>
  <summary>&nbsp;<strong>Cursor / other agents</strong></summary>

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

## Why

| Problem | Kickstart |
|---|---|
| `create-next-app` gives you a blank canvas | Gives you an **architecture** |
| Starter templates get outdated | Context7 fetches **latest docs** every time |
| Setting up biome + lefthook + drizzle + pino takes hours | Done in **seconds** |
| Every project starts differently | Same patterns, **every time** |
| New team members don't know the conventions | `CLAUDE.md` **documents everything** |

---

<p align="center">
  <sub>Built by <a href="https://github.com/LeopoldTr">Leopold Tripot</a> &middot; <a href="LICENSE">MIT License</a></sub>
</p>
