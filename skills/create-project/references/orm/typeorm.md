# TypeORM Reference

Decorator-based ORM — good for NestJS or users from Java/Spring background.

## DataSource Config

```typescript
import { DataSource } from "typeorm";

export const AppDataSource = new DataSource({
  type: "postgres",
  url: process.env.DATABASE_URL,
  entities: ["src/**/*.entity.ts"],
  migrations: ["src/migrations/*.ts"],
  synchronize: false, // NEVER true in production
});
```

## Entity Pattern

```typescript
import { Entity, PrimaryGeneratedColumn, Column } from "typeorm";

@Entity()
export class Example {
  @PrimaryGeneratedColumn("uuid")
  id: string;

  @Column()
  name: string;

  @Column({ type: "timestamp", default: () => "CURRENT_TIMESTAMP" })
  createdAt: Date;
}
```

## Scripts (package.json)

```json
{
  "db:migrate": "typeorm migration:run -d ./src/database/data-source.ts",
  "db:generate": "typeorm migration:generate -d ./src/database/data-source.ts src/migrations/Migration",
  "db:revert": "typeorm migration:revert -d ./src/database/data-source.ts"
}
```

## tsconfig Requirements

TypeORM requires these compiler options:
```json
{
  "emitDecoratorMetadata": true,
  "experimentalDecorators": true
}
```

## Dependencies

- `typeorm` — runtime ORM
- `pg` — PostgreSQL driver
