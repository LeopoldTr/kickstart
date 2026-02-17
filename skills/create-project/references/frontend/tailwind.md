# Tailwind CSS Reference

**Always verify with context7** — Tailwind v4 changed significantly from v3.

## Installation per Framework

| Framework | Plugin | Config |
|-----------|--------|--------|
| Next.js | `@tailwindcss/postcss` | `postcss.config.mjs` |
| React (Vite) | `@tailwindcss/vite` | `vite.config.ts` plugin |
| Vue (Vite) | `@tailwindcss/vite` | `vite.config.ts` plugin |

## Tailwind v4 — CSS-Based Config

No `tailwind.config.js` in v4. Configuration is done in CSS:

```css
@import "tailwindcss";
@plugin "tw-animate-css";

@custom-variant dark (&:is(.dark *));

@theme inline {
  /* CSS variables for theming */
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  /* ... */
}
```

## PostCSS Config (Next.js only)

```javascript
const config = { plugins: { "@tailwindcss/postcss": {} } };
export default config;
```

## Biome Integration

Add to `biome.json` for Tailwind directive support:
```json
{ "css": { "parser": { "tailwindDirectives": true } } }
```
