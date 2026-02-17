# Core Patterns Reference

Patterns shared across all frameworks. **Always verify with context7.**

## biome.json

```json
{
  "$schema": "https://biomejs.dev/schemas/<VERSION>/schema.json",
  "vcs": { "enabled": true, "clientKind": "git", "useIgnoreFile": true },
  "formatter": { "enabled": true, "indentStyle": "space", "indentWidth": 2 },
  "linter": { "enabled": true, "rules": { "recommended": true } },
  "javascript": { "formatter": { "quoteStyle": "double" } },
  "assist": { "enabled": true, "actions": { "source": { "organizeImports": "on" } } }
}
```

For Tailwind projects, add: `{ "css": { "parser": { "tailwindDirectives": true } } }`

## tsconfig.json (base)

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022"],
    "strict": true,
    "verbatimModuleSyntax": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "moduleResolution": "bundler",
    "paths": { "@/*": ["./*"] }
  }
}
```

Framework adjustments:
- **Next.js**: `"jsx": "react-jsx"`, `"lib": ["DOM", "DOM.Iterable", "ES2022"]`, paths `./*`
- **React (Vite)**: `"jsx": "react-jsx"`, `"lib": ["DOM", "DOM.Iterable", "ES2022"]`, paths `./src/*`
- **Vue**: `"jsx": "preserve"`, `"lib": ["DOM", "DOM.Iterable", "ES2022"]`, paths `./src/*`
- **NestJS**: `"module": "commonjs"`, `"emitDecoratorMetadata": true`, `"experimentalDecorators": true`

## lefthook.yml

```yaml
pre-commit:
  commands:
    lint:
      glob: "*.{ts,tsx,vue,css,json}"
      run: bun run lint
```

## Env Validation (zod)

```typescript
import { z } from "zod";

const envSchema = z.object({
  DATABASE_URL: z.string().trim().min(1),    // if DB
  OPENAI_API_KEY: z.string().trim().min(1),  // if AI
  SENTRY_DSN: z.string().url().optional(),   // if Sentry
  LOG_LEVEL: z.string().trim().min(1).optional(),
});

function validateEnv() {
  const result = envSchema.safeParse(process.env);
  if (!result.success) {
    const formatted = result.error.issues
      .map((i) => `  - ${i.path.join(".")}: ${i.message}`)
      .join("\n");
    throw new Error(`Environment validation failed:\n${formatted}`);
  }
  return result.data;
}

export const env = validateEnv();
```

## Error Classes

```typescript
export class AppError extends Error {
  constructor(message: string, public readonly code: string, public readonly statusCode = 500) {
    super(message);
  }
}
export class NotFoundError extends AppError {
  constructor(resource: string, id?: string) {
    super(id ? `${resource} not found: ${id}` : `${resource} not found`, "NOT_FOUND", 404);
  }
}
export class ValidationError extends AppError {
  constructor(message: string) { super(message, "VALIDATION_ERROR", 400); }
}
```

## Result Pattern (neverthrow)

```typescript
export type { Result } from "neverthrow";
export { err, errAsync, ok, okAsync, ResultAsync } from "neverthrow";
```

## Logger (pino)

```typescript
import pino from "pino";

const logger = pino({
  level: process.env.LOG_LEVEL ?? (process.env.NODE_ENV === "production" ? "info" : "debug"),
});

export function createLogger(module: string) {
  return logger.child({ module });
}

export default logger;
```

## .env.example

```bash
# Storage (if DB)
DATABASE_URL=

# AI (if AI features)
OPENAI_API_KEY=

# Sentry (if error monitoring)
SENTRY_DSN=
SENTRY_AUTH_TOKEN=

# Logging (optional)
LOG_LEVEL=
```
