# Next.js Reference

## Architecture

```
project-name/
├── app/                    # App Router pages & layouts
│   ├── [locale]/           # i18n dynamic segment
│   │   ├── layout.tsx      # Locale layout (NextIntlClientProvider)
│   │   ├── page.tsx        # Home page
│   │   └── *.messages.json # Colocated i18n messages
│   ├── globals.css         # Tailwind v4 + CSS variables
│   └── layout.tsx          # Root layout (html, body, fonts)
├── components/             # React components
│   └── ui/                 # shadcn/ui components
├── core/                   # Domain logic by feature
│   └── config/             # Env validation (zod)
├── shared/                 # Cross-cutting utilities
│   ├── errors/             # AppError, NotFoundError, ValidationError
│   └── result/             # neverthrow re-exports (ok, err)
├── infra/                  # Infrastructure adapters
│   ├── logger/             # Pino + AsyncLocalStorage
│   ├── session/            # Cookie-based session reader
│   └── storage/            # DB connection + Drizzle/TypeORM schema
├── i18n/                   # next-intl config
│   ├── routing.ts          # defineRouting (locales, prefix)
│   ├── request.ts          # getRequestConfig + message loading
│   └── navigation.ts       # Typed Link, redirect, usePathname
├── lib/
│   └── utils.ts            # cn() = clsx + tailwind-merge
├── proxy.ts                # Next.js 16 proxy (replaces middleware.ts)
├── biome.json
├── components.json         # shadcn/ui config
├── lefthook.yml
├── next.config.ts
├── package.json
├── postcss.config.mjs
├── tsconfig.json
├── .env.example
└── CLAUDE.md
```

## Key Patterns

### Root Layout (`app/layout.tsx`)

Server component by default. Sets `<html>` and `<body>`, loads fonts, applies dark theme class.

### Proxy (`proxy.ts`)

Next.js 16 convention replacing `middleware.ts`. Composes:
- next-intl middleware for locale detection/routing
- Session cookie setting (UUID via `crypto.randomUUID()`)

### i18n — Colocated Messages

Message files (`.messages.json`) live next to their component. The `i18n/request.ts` file imports and merges them per namespace. Routing uses `localePrefix: "as-needed"` (no prefix for default locale).

### Logger — Request Context

Pino with `AsyncLocalStorage` to inject `requestId` in every log. Use `createLogger(module)` for child loggers.

### Storage — Global Singleton

PostgreSQL connection wrapped in a global singleton to prevent connection leaks during dev HMR:

```typescript
const g = globalThis as unknown as { __sql?: SQL };
export function getSQL(): SQL {
  if (!g.__sql) g.__sql = postgres(DATABASE_URL);
  return g.__sql;
}
```

### PostCSS Config

Next.js uses `@tailwindcss/postcss` plugin (not `@tailwindcss/vite`):

```javascript
const config = { plugins: { "@tailwindcss/postcss": {} } };
export default config;
```

## Scripts (package.json)

```json
{
  "dev": "next dev --turbopack",
  "build": "next build",
  "start": "next start",
  "lint": "biome check .",
  "lint:fix": "biome check --fix .",
  "test": "bun test",
  "prepare": "lefthook install"
}
```

## Conventions

- Server components by default — add `"use client"` only when needed
- Dark theme: slate-950 background, shadcn/ui with slate base
- Colocated tests: `*.test.ts` next to source files
- Double quotes, 2-space indent (biome)
- Strict TypeScript with `verbatimModuleSyntax`
