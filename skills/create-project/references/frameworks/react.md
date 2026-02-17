# React (Vite) Reference

## Architecture

```
project-name/
├── src/
│   ├── app/                    # App shell
│   │   ├── App.tsx             # Root component + router
│   │   ├── routes.tsx          # Route definitions
│   │   └── providers.tsx       # Context providers
│   ├── components/             # Shared components
│   │   └── ui/                 # shadcn/ui components
│   ├── features/               # Feature modules
│   │   └── example/
│   │       ├── components/     # Feature-specific components
│   │       ├── hooks/          # Feature-specific hooks
│   │       ├── store.ts        # Zustand store
│   │       └── index.ts        # Public API
│   ├── shared/                 # Cross-cutting utilities
│   │   ├── errors/             # Error classes
│   │   └── result/             # neverthrow re-exports
│   ├── lib/
│   │   ├── utils.ts            # cn() utility
│   │   ├── logger.ts           # Pino browser logger
│   │   └── i18n.ts             # react-i18next setup
│   ├── locales/                # Translation files
│   │   ├── en/
│   │   └── fr/
│   ├── index.css               # Tailwind v4 + globals
│   └── main.tsx                # Entry point
├── index.html
├── biome.json
├── components.json             # shadcn/ui config
├── lefthook.yml
├── package.json
├── tsconfig.json
├── vite.config.ts
├── .env.example
└── CLAUDE.md
```

## Key Patterns

- **Feature-based structure**: Self-contained modules with components, hooks, store, barrel export
- **Routing**: react-router with centralized route definitions, lazy-loaded for code splitting
- **State**: Zustand stores per feature, colocated in the feature directory
- **i18n**: react-i18next with JSON files per locale/namespace

## Vite Config

```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: { alias: { "@": "/src" } }
});
```

Note: Tailwind v4 with Vite uses `@tailwindcss/vite` plugin (not PostCSS).

## Scripts (package.json)

```json
{
  "dev": "vite",
  "build": "tsc -b && vite build",
  "preview": "vite preview",
  "lint": "biome check .",
  "lint:fix": "biome check --fix .",
  "test": "bun test",
  "prepare": "lefthook install"
}
```
