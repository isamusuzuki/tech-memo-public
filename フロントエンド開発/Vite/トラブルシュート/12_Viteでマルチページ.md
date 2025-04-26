# Viteでマルチページのアプリを作成する

作成日 2025/03/21

## 1. Viteでアプリを作成した直後の状態

```text
--app/
    |--src/
    |   |--components/
    |   |   `--HelloWorld.vue
    |   |--App.vue
    |   `--main.ts
    |--index.html
    `--vite.config.ts
```

- 何も設定しなくても、vite.config.tsがあるフォルダがルートフォルダとして認識される

## 2. マルチページに変更する

公式ガイド => [マルチページアプリ](https://ja.vite.dev/guide/build.html#multi-page-app)

```text
--app/
    |--src/
    |   |--avocado/
    |   |   |--App.vue
    |   |   `--main.ts
    |   `--banana/
    |       |--App.vue
    |       `--main.ts
    |--avocado.html
    |--banana.html
    |--index.html
    `--vite.config.ts
```

- マルチページにジャンプするためのリンク集としてindex.htmlを残すので、ルートフォルダの設定は不要
- ビルドの設定に、エントリーポイントとして複数あるHTMLファイルを指定する
- node:path, node:urlの型を指定するために、`npm add -D @types/node`を実行する

```javascript
import { defineConfig } from 'vite'
import { dirname, resolve } from 'node:path'
import { fileURLToPath } from 'node:url'

import vue from '@vitejs/plugin-vue'
const __dirname = dirname(fileURLToPath(import.meta.url))

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue()],
  server: {
    host: true
  },
  build: {
    rollupOptions: {
      input: {
        main: resolve(__dirname, 'index.html'),
        avocado: resolve(__dirname, 'avocado.html'),
        banana: resuove(__dirname, 'banana.html')
      },
    },
  },
})
```

## 3. HTMLファイルからTypeScriptファイルを読む

avocado.htmlからsrc/avocado/main.tsを読む時は、以下のように記述する必要があった

```html
<script src="src/avocado/main.ts" type="module"></script>
```
