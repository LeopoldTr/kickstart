# shadcn/ui Reference

React-based only (Next.js, React Vite). Not available for Vue.

## components.json

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tailwind": {
    "css": "<CSS_PATH>",
    "baseColor": "slate",
    "cssVariables": true
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui"
  }
}
```

CSS paths:
- Next.js: `app/globals.css`
- React (Vite): `src/index.css`

Set `"rsc": false` for React Vite projects (no server components).

## cn() Utility

Required by shadcn/ui components:

```typescript
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

Dependencies: `clsx`, `tailwind-merge`

## Adding Components

```bash
npx shadcn@latest add <component>
```

Components are copied to `components/ui/` — they're your code, fully customizable.
