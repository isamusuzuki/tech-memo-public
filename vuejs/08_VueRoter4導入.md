# Vue Router 4 を導入する

作成日 2021/10/03

## 01. Vue Router 4 をインストールする

```bash
npm i -D vue-router@4
```

[Installation \| Vue Router](https://next.router.vuejs.org/installation.html)

## 02. 前のバージョンからの変更点

公式ドキュメント => [Migrating from Vue 2 \| Vue Router](https://next.router.vuejs.org/guide/migration/index.html)

- `new Router()` は、 `createRouter()` になった
- 新しい `history` オプションは、`mode` オプションを置き換えた

| mode       | history                  |
| ---------- | ------------------------ |
| `history`  | `createWebHistory()`     |
| `hash`     | `createWebHashHistory()` |
| `abstract` | `createMemoryHistory()`  |

## 03. Vue Router 4 を使いはじめる

サンプルコード

```text
--src/
    |--pages/
    |   |--First.vue
    |   `--Second.vue
    |--router/
    |   `--index.ts
    |--App.vue
    `--index.ts
```

src/router/index.ts

```javascript
import { createRouter, createWebHashHistory } from 'vue-router'
import First from '../pages/First.vue'
import Second from '../pages/Second.vue'

export const routes = [
    { path: '/', redirect: '/first'},
    { path: '/first', component: First },
    { path: '/second', component: Second },
]

export default createRouter({
    history: createWebHashHistory(),
    routes,
})

```

src/App.vue

```html
<template>
    <p>
        <router-link to="/first">First</router-link>
        <router-link to="/second">Second</router-link>
    </p>
    <router-view></router-view>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
})
</script>
```

src/index.ts

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'

const app = createApp(App)

app.use(router)

app.mount('#app')
```

## 04. Composition API と組み合わせて使う

公式ドキュメントを写経する

[Vue Router and the Composition API \| Vue Router](https://next.router.vuejs.org/guide/advanced/composition-api.html)

```javascript
import { useRouter, useRoute } from 'vue-router'

export default {
    const router = useRouter()
    const route = useRoute()

    const pushWithQuery = (query) => {
        router.push({
            name: 'search',
            query: {
                ...route.query,
            }
        })
    }
}
```

```javascript
import { useRoute } from 'vue-router'
import { ref, watch } from 'vue'

export default {
    const route = useRoute()
    const userData = ref()

    watch(
        () => route.params.id,
        async newId => {
            userDate.value = await fetchUser(newId)
        }
    )

}
```
