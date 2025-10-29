# Unpluginとは

作成日 2025/10/29

## 1. 公式ガイド（英語）を読む

[Getting Started | Unplugin](https://unplugin.unjs.io/guide/)

Unpluginは、Viteなどの、さまざまなビルドツールに対する、統一したプラグインシステムを提供するライブラリです

## 2. [unplugin-vue-components](https://unplugin.unjs.io/showcase/unplugin-vue-components.html)

最新版 v30.0.0

Vueのための、コンポーネントの、オンデマンド自動インポート

インストール => `npm i unplugin-vue-components -D`

設定 => vite.config.ts

```javascript
import { defineConfig } from 'vite';
import Components from 'unplugin-vue-components/vite';

export default defineConfig({
  plugins: [
    Components({ /* options */ }),
  ],
})
```

使い方 => いつものようにtemplateパートにコンポーネントを書く。そのコンポーネントがオンデマンドでインポートされる

Vuetify, Ant Design Vue, Element Plusのような人気のUIライブラリのリゾルバをビルトインで用意している

```javascript
import { defineConfig } from 'vite';
import Components from 'unplugin-vue-components/vite';
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers';

export default defineConfig({
  plugins: [
    Components({
        resolvers: [ElementPlusResolver()],
    })
  ]
});
```

## 3. [unplugin-auto-import](https://unplugin.unjs.io/showcase/unplugin-auto-import.html)

最新版 v20.2.0

オンデマンドの、APIの自動インポートで、TypeScriptをサポートしている

```javascript
// このインポート行が不要になる
import { computed, ref } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)
```

Vue専用ではなくて、Reactでも有効

```javascript
// このインポート行が不要になる
import { useState } from 'react'

export function Counter() {
  const [count, setCount] = useState(0)
  return <div>{ count }</div>
}
```

インストール => `npm i -D unplugin-auto-import`

設定 => vite.config.ts

```javascript
import { defineConfig } from 'vite'
import AutoImport from 'unplugin-auto-import/vite'

export default defineConfig({
  plugins: [
    AutoImport({ /* options */ }),
  ],
})
```
