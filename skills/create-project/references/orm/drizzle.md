# Drizzle ORM Reference

Recommended ORM — lightweight, SQL-like, excellent TypeScript inference.

## drizzle.config.ts

```typescript
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  schema: "<SCHEMA_PATH>",
  out: "./drizzle",
  dialect: "postgresql",
  dbCredentials: { url: process.env.DATABASE_URL ?? "" }
});
```

Schema paths:
- Next.js: `./infra/storage/schema.ts`
- NestJS: `./src/database/schema.ts`

## Connection — Global Singleton (for dev HMR)

```typescript
import postgres from "postgres";

const g = globalThis as unknown as { __sql?: SQL };
export function getSQL(): SQL {
  if (!g.__sql) g.__sql = postgres(DATABASE_URL);
  return g.__sql;
}

export const db = drizzle(getSQL());
```

## Scripts (package.json)

```json
{
  "db:generate": "drizzle-kit generate",
  "db:migrate": "drizzle-kit migrate",
  "db:studio": "drizzle-kit studio"
}
```

## Dependencies

- `drizzle-orm` — runtime ORM
- `postgres` — PostgreSQL driver
- `drizzle-kit` — devDependency, CLI for migrations and studio
