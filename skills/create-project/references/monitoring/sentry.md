# Sentry Reference

Error tracking, performance monitoring, session replay.

**Always use context7** — Sentry SDK setup changes frequently per framework.

## SDK per Framework

| Framework | Package |
|-----------|---------|
| Next.js | `@sentry/nextjs` |
| NestJS | `@sentry/nestjs` |
| React (Vite) | `@sentry/react` |
| Vue | `@sentry/vue` |

## Env Variables

```bash
SENTRY_DSN=
SENTRY_AUTH_TOKEN=  # for source maps upload
```

Add to env validation schema:
```typescript
SENTRY_DSN: z.string().url().optional(),
```

## Key Features to Configure

- **Error boundaries** (React/Vue) — catch rendering errors
- **Server-side error capture** (Next.js/NestJS) — catch API errors
- **Source maps upload** — via build plugin or CI step
- **Performance monitoring** — tracing for API routes
- **Session replay** (frontend) — optional, for debugging UX issues

## Integration Notes

- Next.js: requires `sentry.client.config.ts`, `sentry.server.config.ts`, `sentry.edge.config.ts`, and `next.config.ts` wrapper
- NestJS: global exception filter integration
- React/Vue: wrap app root with error boundary, configure in entry point
