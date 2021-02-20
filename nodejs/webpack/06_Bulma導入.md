# Bulma 導入

作成日 2021/02/17、更新日 2021/02/20

## 01. とりあえず公式サイトを写経してみる

[With webpack \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/customize/with-webpack/)

新しいフォルダを用意する

```bash
mkdir bacon
cd bacon
npm init -y
```

必要なモジュールをインストールする

```bash
npm i -D webpack webpack-cli webpack-dev-server
npm i -D style-loader css-loader
npm i -D sass-loader node-sass
npm i -D bulma
npm i -D mini-css-extract-plugin
```

以下のような構成でファイルを用意する

```text
--bacon/
    |--dist/
    |   `--index.html
    |--src/
    |   |--index.js
    |   `--mystyles.scss
    `--webpack.config.js
```

dist/index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <title>Bacon</title>
    <link rel="stylesheet" href="css/mystyles.css" />
  </head>

  <body>
    <section class="section">
      <div class="container">
        <h1 class="title">Bulma</h1>
        <h2 class="subtitle">Modern CSS framework</h2>
        <div class="field">
          <div class="control">
            <input class="input" type="text" placeholder="Input you name" />
          </div>
        </div>
        <div class="field">
          <p class="control">
            <span class="select">
              <select>
                <option>Select dropdown</option>
              </select>
            </span>
          </p>
        </div>
        <div class="buttons">
          <a class="button is-primary">Primary</a>
          <a class="button is-link">Link</a>
        </div>
      </div>
    </section>
  </body>
</html>
```

src/index.js

```javascript
require('./mystyles.scss');
```

src/style.scss

```css
@charset "utf-8";
@import '~bulma/bulma';
```

webpack.config.js

```javascript
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

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
        test: /\.scss$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: 'css-loader',
          },
          {
            loader: 'sass-loader',
          },
        ],
      },
    ],
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/mystyles.css',
    }),
  ],
};
```

### mini-css-extract-plugin の役割とは

CSS を別ファイルに仕立ててくれること

[webpack\-contrib/mini\-css\-extract\-plugin: Lightweight CSS extraction plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)

## 02. 個人的な感想

Bulma は、js コードの中に埋め込まれるのではなく、Head タグの冒頭で読まれる必要があると思った。そのために、mini-css-extract-plugin が必要なのではないか？

現時点では、TypeScript コードの場合の Bulma の組み込み方がわからない。それがわかったたとしても、「Head タグに CDN を書き込めばよくね？」に反論できそうにない

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/bulma@0.9.1/css/bulma.min.css"
/>
```

## 03. とにかくカンタンに Bulma を導入する

すでに Webpack が導入済みで、Vue の単一コンポーネントファイルも使えるようになっているものとする

Bulma だけをインストールする

```bash
npm i -D bulma
```

src/components/App.vue

```html
<template>
  <section class="section">
    <div class="container">
      <h1 class="title">Hello, {{ name }}!!</h1>
      <div class="field">
        <div class="control">
          <input
            class="input"
            type="text"
            placeholder="お名前を入力してください"
          />
        </div>
      </div>
      <div class="field">
        <button class="button is-primary">送信</button>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
  import Vue from 'vue';
  export default Vue.extend({
    data() {
      return {
        name: 'Bacon',
      };
    },
  });
</script>

<style>
  @import '../../node_modules/bulma/css/bulma.min.css';
</style>
```

これで、Bulma も JS コードの中にバンドルされる

### Bulmaをバンドルしたら、すぐに警告が出るようになった

```text
WARNING in asset size limit: The following asset(s) exceed the recommended size limit (244 KiB).
This can impact web performance.
Assets: 
  index.bundle.js (300 KiB)
```

デフォルトのWebpackのパフォーマンスヒントでは、244KB以上のバンドルファイルに警告が出る

やっぱり「Head タグに CDN を書き込めばよくね？」に戻ってしまった
