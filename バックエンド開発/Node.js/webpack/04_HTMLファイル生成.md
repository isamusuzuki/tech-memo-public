# HTML ファイルを生成する

作成日 2021/02/10、更新日 2021/02/19

## 01. html-webpack-plugin とは

HTML ファイルの生成してくれるプラグイン

[HtmlWebpackPlugin \| webpack](https://webpack.js.org/plugins/html-webpack-plugin/)

[jantimon/html\-webpack\-plugin: Simplifies creation of HTML files to serve your webpack bundles](https://github.com/jantimon/html-webpack-plugin)

モジュールをインストールする

```bash
npm i -D html-webpack-plugin
```

## 02. 一番簡単な設定例

webpack.config.js

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    index: './src/index.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [new HtmlWebpackPlugin()],
};
```

これでバンドルすると、以下の HTML ファイルが生成されている

src/index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Webpack App</title>
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <script defer="defer" src="index.bundle.js"></script>
  </head>

  <body></body>
</html>
```

## 03. テンプレートファイルを使った設定例

HTML ファイルを自動生成してくれるのはありたいが、body タグの中に`<div id="app"></div>`を用意しておかないと、Vue.js がうまく動かない。少しカスタマイズしたい

webpack.config.js

```javascript
module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: './templates/index.html',
      inject: true,
    }),
  ],
};
```

templates/index.html

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <title>Apple</title>
  </head>

  <body>
    <div id="app"></div>
  </body>
</html>
```

- head タグの一番下に、`<script defer="defer" src="index.bundle.js"></script>` が挿入された形でファイルが生成された

### index.html以外のファイル名にしたい場合

html-webpack-plugin は、デフォルトで `index.html` ファイルを生成する

違うファイル名を生成したいときは、`filename` オプションで指定する

```javascript
module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'about.html',
      template: './templates/about.html',
      inject: true,
    }),
  ],
};
```

### 複数のファイルを生成したい場合

plugin に、もうひとつ `new HtmlWebpackPlugin()` を書く

```javascript
module.exports = {
  plugins: [
    new HtmlWebpackPlugin(
      template: './templates/index.html',
      inject: true,
    ),
    new HtmlWebpackPlugin({
      filename: 'about.html',
      template: './templates/about.html',
      inject: true,
    }),
  ],
};
```
