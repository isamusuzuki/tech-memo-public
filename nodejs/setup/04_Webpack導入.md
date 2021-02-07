# Webpack を導入して、Vue.js の開発を TypeScript で行う

作成日 2021/02/07

## 01. Webpack 導入

### 01-1. Webpack とは

モジュールのバンドラー。ローダーを組み合わせることで、様々なタイプのファイルを変換できるようになる

[Concepts \| webpack](https://webpack.js.org/concepts/)

### 01-2. 最もシンプルな形で Webpack を使う

新しいプロジェクトを作成する

```bash
cd ~
mkdir banana
cd banana
npm init -y
# => package.json が生成される
```

必要なモジュールをインストールする

```bash
npm i -D webpack webpack-cli webpack-dev-server
```

以下の構成でファイルを用意する

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   `--index.js
    |--package.json
    `--webpack.config.js
```

dist/index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <title>Banana</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="index.bundle.js"></script>
  </body>
</html>
```

src/index.js

```javascript
(() => {
  const app = document.getElementById('app');
  app.innerText = 'Hello, Banana!';
})();
```

package.json

```json
{
  // ここより前は省略
  "scripts": {
    "dev": "webpack-cli serve --mode=development",
    "build": "webpack --mode=production"
  }
  // ここより後も省略
}
```

webpack.config.js

```javascript
const path = require('path');

module.exports = {
  entry: {
    index: './src/index.js'
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devServer: {
    contentBase: path.resolve(__dirname, 'dist')
    compress: true,
    port: 8080
  }
};
```

webpack を動かす

```bash
# 開発サーバーを動かす
npm run dev
```

- `http://localhost:8080` で、ページが表示される
- `http://localhost:8080/webpack-dev-server` で、開発サーバーが提供しているページの一覧が表示される

```bash
# JSファイルをバンドルする
npm run build
# => dist/index.bundle.js が生成される
```

## 02. Vue Loader 導入

### 02-1. Vue Loader とは

Vue.js をバンドルするローダー

[Introduction · vue\-loader](https://vue-loader-v14.vuejs.org/ja/)

### 02-2. 最もシンプルな形で Vue Loader を使う

必要なモジュールをインストールする

```bash
npm i -D vue vue-loader vue-template-compiler
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.vue    // <= 追加
    |   `--index.js       // <= 全部書換
    |--package.json
    `--webpack.config.js  // <= 追記
```

src/components/App.vue

- 単一ファイルコンポーネントを書く

```html
<template>
  <div>
    <p>Hello, {{name}}!!</p>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data: () => {
      return {
        name: 'Banana'
      };
    }
  };
</script>
```

src/index.js

```javascript
import Vue from 'vue';
import App from './components/App';

new Vue({
  el: '#app',
  components: { App },
  template: '<app/>'
});
```

webpack.config.js

```javascript
// 冒頭に追加する
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
  // 以下を追加する
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.vue'],
    alias: {
      vue$: 'vue/dist/vue.esm.js'
    }
  },
  plugins: [new VueLoaderPlugin()]
};
```

## 03. css-loader, style-loader 導入

### 03-1. css-loader, style-loader とは

- css-loader ... JavaScript の中で CSS を扱えるようにする
- style-loader ... JavaScript の中にある CSS を DOM に挿入する

この 2 つは組み合わせて使うことが多い

### 03-2. css-loader, style-loader を組み込む

必要なモジュールをインストールする

```bash
npm i -D css-loader style-loader
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.vue    // <= 追記
    |   `--index.js
    |--package.json
    `--webpack.config.js  // <= 追記
```

src/components/App.vue

```html
<!-- // 一番下に追加する -->
<style scoped>
  .title {
    font-size: 48px;
  }
</style>
```

webpack.config.js

```javascript
module.exports = {
  module: {
    rules: [
      // 以下を追加する
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

## 04. ts-loader 導入

### 04-1. ts-loader とは

TypeScript を取り扱うローダー

[TypeStrong/ts\-loader: TypeScript loader for webpack](https://github.com/TypeStrong/ts-loader)

### 04-2. ts-loader を組み込む

必要なモジュールをインストールする

```bash
npm i -D typescript ts-loader

npx tsc --init
# => tsconfig.json が生成される
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.vue     // <= 一部書換
    |   |--index.ts        // <= 全面書換
    |   `--vue-shims.d.ts  // <= 追加
    |--package.json
    |--tsconfig.json       // <= 追加
    `--webpack.config.js   // <= 追記
```

src/components/App.vue

```html
<!-- // 以下を書き換える -->
<script lang="ts">
  import Vue from 'vue'

  export default Vue.extend({
    data() {
      return {
        name: 'Banana'
      }
    }
  })
</script>
```

src/index.ts

- JavaScript を TypeScript に書き換える
- 単一ファイルコンポーネントを読み込むときは `.vue` の拡張子まで書く

```javascript
import Vue from 'vue'
import App from './components/App.vue'

new Vue({
  el: '#app',
  components: { App },
  template: '<app/>'
})
```

src/vue-shims.d.ts

- 単一ファイルコンポーネントの型定義を TypeScript に教える

```javascript
declare module "*.vue" {
  import Vue from 'vue'
  export default Vue
}
```

tsconfig.json

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "es2015",
    "strict": true,
    "noImplicitReturns": true,
    "moduleResolution": "node"
  }
}
```

webpack.config.js

```javascript
module.exports = {
  module: {
    rules: [
      // 以下を書き加える
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          appendTsSuffixTo: [/\.vue$/]
        }
      }
    ]
  },
  resolve: {
    // '.ts'を書き加える
    extensions: ['.js', '.ts', '.vue'],
    alias: {
      vue$: 'vue/dist/vue.esm.js'
    }
  }
};
```

## 05. TypeScript 寄りのコードの書き方をする

必要なモジュールをインストールする

```bash
npm i -D vue-class-component vue-property-decorator
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.ts      // <= 全面書換
    |   |--index.ts        // <= 一部書換
    |   `--vue-shims.d.ts
    |--package.json
    |--tsconfig.json       // <= 追記
    `--webpack.config.js
```

src/components/App.ts

```javascript
import Vue from 'vue'
import Component from 'vue-class-component'

@Component({
  template: `
    <div>
      <p class="title">Hello, {{ name }}!!!</p>
    </div>
  `
})
export default class App extends Vue {
  name: string = 'Banana'
}
```

src/index.ts

```javascript
// 以下を書き換える
// import App from './components/App.vue'
import App from './components/App'
```

tsconfig.json

```json
{
  "compilerOptions": {
    // 以下を書き加える
    "experimentalDecorators": true
  }
}
```
