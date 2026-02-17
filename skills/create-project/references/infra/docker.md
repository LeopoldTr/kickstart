# Docker Reference

## Dockerfile (multi-stage, bun)

```dockerfile
FROM oven/bun:latest AS base
WORKDIR /app

# Install dependencies
FROM base AS deps
COPY package.json bun.lock ./
RUN bun install --frozen-lockfile

# Build
FROM base AS build
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN bun run build

# Production
FROM base AS production
ENV NODE_ENV=production
COPY --from=deps /app/node_modules ./node_modules
COPY --from=build /app/<BUILD_OUTPUT> ./<BUILD_OUTPUT>
COPY package.json ./
EXPOSE 3000
CMD ["bun", "run", "start"]
```

Build output per framework:
- Next.js: `.next/` + `public/`
- NestJS: `dist/`
- React/Vue (Vite): `dist/` (serve with static server)

## .dockerignore

```
node_modules
.git
.env
.env.local
*.log
dist
.next
coverage
```

## docker-compose.yml

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    env_file: .env
    depends_on:
      - db  # if DB selected

  db:  # if DB selected
    image: postgres:17-alpine
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: app
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:  # if DB selected
```
