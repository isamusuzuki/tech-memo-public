# Webpack 導入

作成日 2021/02/10

## 01. ゴール

Vue.js の開発を TypeScript で行い、Webpack でバンドルする

## 02. Webpack とは

モジュールのバンドラー。ローダーを組み合わせることで、様々なタイプのファイルを変換できるようになる

[Concepts \| webpack](https://webpack.js.org/concepts/)

## 03. 最もシンプルな形で Webpack を使う

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
