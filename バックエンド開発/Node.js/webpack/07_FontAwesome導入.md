# FontAwesome 導入

作成日 2021/02/17、更新日 2021/02/21

## 01. とりあえず参考記事と公式サイトを写経してみる

[1 年後に差がつく Font Awesome5 ～フロントエンド開発\(ES6,Webpack4,Babel7\)への導入～ \- Qiita](https://qiita.com/riversun/items/4faa56ac40071f638313)

[SVG JavaScript Core \| Font Awesome](https://fontawesome.com/how-to-use/on-the-web/advanced/svg-javascript-core)

新しいフォルダを用意する

```bash
mkdir coconut
cd coconut
npm init -y
```

必要なモジュールをインストールする

```bash
npm i -D webpack webpack-cli webpack-dev-server
npm i -D @fortawesome/fontawesome-svg-core
npm i -D @fortawesome/free-solid-svg-icons
npm i -D @fortawesome/free-regular-svg-icons
```

以下のような構成でファイルを用意する

```text
--coconut/
    |--dist/
    |   `--index.html
    |--src/
    |   `--index.js
    `--webpack.config.js
```

dist/index.html

```html
<!DOCTYPE html>
<html lang="js">
  <head>
    <meta charset="UTF-8" />
    <title>Coconut</title>
    <script defer src="index.bundle.js"></script>
  </head>
  <body>
    <i class="fas fa-dog fa-3x"></i>
    <i class="far fa-comments fa-3x"></i>
    <i class="fas fa-cat fa-3x fa-flip-horizontal"></i>
  </body>
</html>
```

src/index.js

```javascript
import { dom, library } from '@fortawesome/fontawesome-svg-core';
import { faDog, faCat } from '@fortawesome/free-solid-svg-icons';
import { faComments } from '@fortawesome/free-regular-svg-icons';

library.add(faDog, faComments, faCat);

dom.watch();
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

## 02. FontAwesomeもバンドルしたら、警告が出た

デフォルトのWebpackのパフォーマンスヒントでは、244KB以上のバンドルファイルに警告が出る

Bulma同様、Head タグに CDN を書き込んだほうがいいのかもしれない
