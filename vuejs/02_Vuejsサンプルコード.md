# Vue.js サンプルコードとその実行

作成日 2022/12/15

## サンプルコードの用意

```text
--avocado/
  |--public/
  |   `--index.html
  |--src/
  |   |--App.vue
  |   |--index.ts
  |   `--shims-vue.d.ts
  `--webpack.config.js
```

public/index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <title>Avocado</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="index.bundle.js"></script>
  </body>
</html>
```

src/shims-vue.d.ts

```javascript
/* eslint-disable */
declare module '*.vue' {
    import type { DefineComponent } from 'vue'
    const component: DefineComponent<{}, {}, any>
    export default component
}
```

src/App.vue

```html
<template>
  <h1>Avocado</h1>
  <h2>{{ programVersion }}</h2>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  export default defineComponent({
    name: 'App',
    data() {
      return {
        programVersion: 'v0000 開発中',
      };
    },
  });
</script>
```

src/index.ts

```javascript
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app');
```

webpack.config.js

```javascript
const path = require('path');
const { VueLoaderPlugin } = require('vue-loader');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.ts',
  },
  output: {
    path: path.resolve(__dirname, 'public'),
    filename: '[name].bundle.js',
  },
  devServer: {
    static: {
      directory: path.resolve(__dirname, 'public'),
    },
    compress: true,
    port: 8080,
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
      },
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          appendTsSuffixTo: [/\.vue$/],
        },
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  resolve: {
    extensions: ['.js', '.ts', '.vue'],
  },
  plugins: [new VueLoaderPlugin()],
};
```

## スクリプトの実行

```bash
# テストサーバーの起動
npx webpack serve --config=webpack.config.js

# または、package.json に追記しておいてから
npm run dev
```

package.json

```json
{
  "scripts": {
    "dev": "webpack serve --config=webpack.config.js"
  }
}
```
