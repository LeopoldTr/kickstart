# LottieFiles Reference

Lottie animations for rich UI — loaders, icons, onboarding illustrations, micro-interactions.

## When to Suggest

Suggest for projects with: landing pages, dashboards, onboarding flows, or anywhere that benefits from lightweight vector animations.

## Libraries per Framework

| Framework | Library | Import |
|-----------|---------|--------|
| React (Next.js, Vite) | `lottie-react` | `import Lottie from "lottie-react"` |
| Vue | `vue3-lottie` | `import { Vue3Lottie } from "vue3-lottie"` |

## Usage Pattern (React)

```tsx
import Lottie from "lottie-react";
import loadingAnimation from "@/assets/animations/loading.json";

export function LoadingSpinner() {
  return <Lottie animationData={loadingAnimation} loop className="w-24 h-24" />;
}
```

## Usage Pattern (Vue)

```vue
<script setup lang="ts">
import { Vue3Lottie } from "vue3-lottie";
import loadingAnimation from "@/assets/animations/loading.json";
</script>

<template>
  <Vue3Lottie :animation-data="loadingAnimation" :loop="true" class="w-24 h-24" />
</template>
```

## Assets

Store Lottie JSON files in `assets/animations/`. Get free animations from [lottiefiles.com](https://lottiefiles.com).
