# Vue Loader 導入

作成日 2021/02/10

## 01. Vue Loader とは

Vue.js をバンドルするローダー

[Introduction · vue\-loader](https://vue-loader-v14.vuejs.org/ja/)

## 02. 最もシンプルな形で Vue Loader を使う

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
    data() {
      return {
        name: 'Banana',
      };
    },
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
  template: '<app/>',
});
```

webpack.config.js

```javascript
const path = require('path');
// 以下を追加する
const VueLoaderPlugin = require('vue-loader/lib/plugin');
// ここまで

module.exports = {
  entry: {
    index: './src/index.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  devServer: {
    contentBase: path.resolve(__dirname, 'dist'),
    compress: true,
    port: 8080,
  },
  // 以下を追加する
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
      },
    ],
  },
  resolve: {
    extensions: ['.js', '.vue'],
    alias: {
      vue$: 'vue/dist/vue.esm.js',
    },
  },
  plugins: [new VueLoaderPlugin()],
  // ここまで
};
```

## 03. 単一ファイルコンポーネントの中で CSS を書く

### css-loader, style-loader とは

- css-loader ... JavaScript の中で CSS を扱えるようにする
- style-loader ... JavaScript の中にある CSS を DOM に挿入する

この 2 つは組み合わせて使うことが多い

### css-loader, style-loader を組み込む

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
<template>
  <div>
    <!-- // class 属性を追加する -->
    <p class="title">Hello, {{name}}!!</p>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        name: 'Banana',
      };
    },
  };
</script>

<!-- // 追加する -->
<style scoped>
  .title {
    font-size: 48px;
  }
</style>
```

webpack.config.js

```javascript
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
  entry: {
    index: './src/index.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  devServer: {
    contentBase: path.resolve(__dirname, 'dist'),
    compress: true,
    port: 8080,
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
      },
      // 以下を追加する
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      // ここまで
    ],
  },
  resolve: {
    extensions: ['.js', '.vue'],
    alias: {
      vue$: 'vue/dist/vue.esm.js',
    },
  },
  plugins: [new VueLoaderPlugin()],
};
```
