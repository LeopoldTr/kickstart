# Vue.js Reference

## Architecture

```
project-name/
├── src/
│   ├── app/                    # App shell
│   │   ├── App.vue             # Root component
│   │   ├── router.ts           # Vue Router config
│   │   └── main.ts             # Entry point + plugin setup
│   ├── components/             # Shared components
│   │   └── ui/                 # UI component library
│   ├── features/               # Feature modules
│   │   └── example/
│   │       ├── components/     # Feature-specific components
│   │       ├── composables/    # Feature-specific composables
│   │       ├── store.ts        # Pinia store
│   │       └── index.ts        # Public API
│   ├── shared/                 # Cross-cutting utilities
│   │   ├── errors/             # Error classes
│   │   └── result/             # neverthrow re-exports
│   ├── lib/
│   │   ├── utils.ts            # Utility functions
│   │   ├── logger.ts           # Pino browser logger
│   │   └── i18n.ts             # vue-i18n setup
│   ├── locales/                # Translation files
│   │   ├── en/
│   │   └── fr/
│   ├── styles/
│   │   └── main.css            # Tailwind v4 + globals
│   └── env.d.ts                # Vite env types
├── index.html
├── biome.json
├── lefthook.yml
├── package.json
├── tsconfig.json
├── vite.config.ts
├── .env.example
└── CLAUDE.md
```

## Key Patterns

- **Composition API only**: `<script setup lang="ts">` exclusively, no Options API
- **Feature-based structure**: Self-contained modules with components, composables, store, barrel export
- **State**: Pinia stores per feature (setup syntax), colocated in feature directory
- **Routing**: Vue Router with lazy-loaded feature routes for code splitting
- **i18n**: vue-i18n with JSON files per locale/namespace

## Vite Config

```typescript
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [vue(), tailwindcss()],
  resolve: { alias: { "@": "/src" } }
});
```

## Scripts (package.json)

```json
{
  "dev": "vite",
  "build": "vue-tsc -b && vite build",
  "preview": "vite preview",
  "lint": "biome check .",
  "lint:fix": "biome check --fix .",
  "test": "bun test",
  "prepare": "lefthook install"
}
```
