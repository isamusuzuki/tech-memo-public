# VueUseとは？

作成日 2025/11/12

## 1. 公式サイト（英語）を読む

[VueUse](https://vueuse.org/)

> Collection of Essential Vue Composition Utilities

[Get Started | VueUse](https://vueuse.org/guide/)

インストール => `npm i @vueuse/core`

## 2. サンプルコードを読む

[vueuse/vueuse-vite-starter: ⚡️ Starter for Vite + VueUse + TypeScript](https://github.com/vueuse/vueuse-vite-starter)

Live Demo => `https://vueuse-vite-starter.netlify.app/`

```html
<template>
    <h3>Mouse: {{x}} x {{y}}</h3>
    <h3>
        Counter: {{count}}
        <a @click='inc()' style='margin-right:10px'>+</a>
        <a @click='dec()'>-</a>
    </h3>
</template>
<script setup lang="ts">
import { useMouse, useCounter } from '@vueuse/core'

const { x, y } = useMouse()
const { count, inc, dec } = useCounter()
</script>
```

## 3. [useFetch](https://vueuse.org/core/useFetch/)

```javascript
import { useFetch } from '@vueuse/core'

const { isFetching, error, data } = useFetch(url)
```
