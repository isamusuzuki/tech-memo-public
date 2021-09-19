# Vue.js 3 の開発環境を構築する

作成日 2021/09/18

## 01. Vue.js 3.x への移行を開始する

今まで、Vue.js 2.x + TypeScript + Webpack の組み合わせで、Web クライアントを開発してきたが、Vue.js 3.x へ移行することにした。「もう一度イチから勉強」とはならないだろうが、たくさん学ぶことがあるだろう

## 02. Vue.js のバージョンに影響されない部分の構築

- まっさらな状態から開発環境を構築する
- ディレクトリ名の `avocado` は仮の名前
- `npm i -D {name}` は、`npm install --save-dev {name}` の短縮形

```bash
node -v
# => v14.17.6
npm -v
# => 7.23.0

# 新規プロジェクトを作成する
cd ~
mkdir avocado
cd avocado

# package.json ファイルを生成する
npm init -y
```

### TypeScript 導入

```bash
npm i -D typescript
npx tsc --init
# => tsconfig.json ファイルが生成される
```

### tsconfig.json の設定例

```json
{
  "compilerOptions": {
    "target": "ES2019",
    "module": "es2015",
    "moduleResolution": "node",
    "sourceMap": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitReturns": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts", "src/**/*.vue"]
}
```

### ESLint 導入

※ 前提として、VSCode の拡張機能としての ESLint はインストールしない

```bash
npm i -D eslint
npx eslint --init
# => インタラクティブなセットアップが始まる
# => 回答が終わるとモジュールの追加インストールが始まり、
# => .eslintrc.json ファイルが生成される
```

### セットアップの回答例

```text
? How would you like to use ESLint?
  To check syntax only
> To check syntax and find problems
  To check syntax, find problems, and enforce code style

? What type of modules does your project use?
> JavaScript modules (import/export)
  CommonJS (require/exports)
  Non of these

? Which framework does your project use?
  React
> Vue.js
  None of these

? Does your project use TypeScript?
  No
> Yes

? Where does your code run?
> Browser
> Node

? What format do you want your config file to be in?
  JavaScript
  YAML
> JSON

? Would you like to install them now with npm?
  No
> Yes
```

### Webpack 導入

```bash
npm i -D webpack webpack-cli webpack-dev-server
npm i -D ts-loader
npm i -D css-loader style-loader
```

## 03. Vue.js 3.x で開発するための構築

```bash
# バージョン2系の場合
# npm i -D vue
# npm i -D vue-loader
# npm i -D vue-template-compiler

# バージョン3系の場合
npm i -D vue@next
npm i -D vue-loader@next
npm i -D @vue/compiler-sfc
```

- Vue.js 本体は、`vue` を `vue@next` に置き換える
- Vue.js 用の Webpack ローダーは、`vue-loader` を `vue-loader@next` に置き換える
- Webpack が SFC を取り扱えるようにするツールは、`vue-template-compiler`　を `@vue/compiler-sfc` に置き換える。（SFC = Single File Components 単一ファイルコンポーネント）

## 04. サンプルコードの用意とスクリプトの実行

### サンプルコードの用意

```text
--avocado/
  |--public/
  |   `--index.html
  |--src/
  |   |--App.vue
  |   |--index.ts
  |   `--shims-vue.d.ts
  `--webpack.config.json
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

src/shims-vue.d.ts

```javascript
/* eslint-disable */
declare module '*.vue' {
    import type { DefineComponent } from 'vue'
    const component: DefineComponent<{}, {}, any>
    export default component
}
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

### スクリプトの実行

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
