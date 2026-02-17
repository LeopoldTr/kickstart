# NestJS Reference

## Architecture

```
project-name/
├── src/
│   ├── modules/                # Feature modules
│   │   └── example/
│   │       ├── example.module.ts
│   │       ├── example.controller.ts
│   │       ├── example.service.ts
│   │       ├── example.repository.ts
│   │       └── example.validation.ts   # Zod schemas
│   ├── common/                 # Shared utilities
│   │   ├── errors/             # AppError, NotFoundError, ValidationError
│   │   ├── result/             # neverthrow re-exports
│   │   ├── filters/            # Exception filters
│   │   └── pipes/              # Zod validation pipe
│   ├── config/                 # Env validation (zod)
│   │   └── index.ts
│   ├── database/               # DB setup (Drizzle or TypeORM)
│   │   ├── database.module.ts
│   │   ├── db.ts               # Connection (Drizzle) or data-source.ts (TypeORM)
│   │   └── schema.ts           # Drizzle schema or entities/ dir (TypeORM)
│   ├── logger/                 # Pino logger module
│   │   └── logger.module.ts
│   ├── app.module.ts           # Root module
│   └── main.ts                 # Bootstrap
├── biome.json
├── lefthook.yml
├── nest-cli.json
├── package.json
├── tsconfig.json
├── tsconfig.build.json
├── .env.example
└── CLAUDE.md
```

## Key Patterns

### Layer Separation

```
Controller → Service → Repository → Database
     ↓          ↓
   Zod       Business
  schema      logic
```

- **Controllers**: HTTP layer, validate input (zod pipe), delegate to service
- **Services**: Business logic, orchestration, no direct DB calls
- **Repositories**: Data access only, no business logic

### Zod Validation Pipe

Custom NestJS pipe using zod for request validation instead of class-validator:

```typescript
@Injectable()
export class ZodValidationPipe implements PipeTransform {
  constructor(private schema: ZodSchema) {}
  transform(value: unknown) {
    const result = this.schema.safeParse(value);
    if (!result.success) throw new BadRequestException(result.error.issues);
    return result.data;
  }
}
```

### Exception Filter

Global exception filter that maps `AppError` subclasses to HTTP responses with proper status codes.

## Scripts (package.json)

```json
{
  "dev": "nest start --watch",
  "build": "nest build",
  "start": "node dist/main",
  "start:prod": "node dist/main",
  "lint": "biome check .",
  "lint:fix": "biome check --fix .",
  "test": "bun test",
  "prepare": "lefthook install"
}
```

## Conventions

- One module per feature domain
- Zod for validation (not class-validator/class-transformer)
- Result pattern (neverthrow) for service layer error handling
- Structured logging with Pino
- Strict TypeScript
