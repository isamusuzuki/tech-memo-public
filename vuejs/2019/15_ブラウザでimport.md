# ブラウザで import する

作成日 2018/12/20、更新日 2019/01/09

## 拡張子`.mjs`はエラーになった

ブラウザが「MIME タイプが空っぽのファイルは危険だから読み込まない」とエラーを出した

素直に拡張子を`.js`に戻せば動作するが、どう設定すればいいか知りたくなった

### Apache における MIME タイプの設定方法

http.conf ファイルで TypesConfig を検索してみた

=> `TypesConfig conf/mime.type`という行を発見

=> `C:\xampp\apache\conf\mime.types`ファイルを発見、1,855 行もある

=> `application/javascript js`という行を発見、確かに`mjs`はなかった

=> httpd.conf に「AddType MIME タイプ 拡張子」で追加すれば解決できる

[MIME タイプの追加\(AddType\) \- コンテンツの設置 \- Apache 入門](https://www.adminweb.jp/apache/docroot/index4.html)

## import 機能について

ES6 の機能である import をブラウザがすでにサポートしているならば、
webpack によるコンパイルなしでも、複数ファイルから成る Vue.js アプリケーションが動くのではないか？

[import \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)

> The import statement cannot be used in embedded scripts unless such script has a type="module".

[JavaScript modules via script tag](https://caniuse.com/#feat=es6-module)

```html
<script type="module"></script>
```

### Google の JS modules 説明ブログを発見

[Using JavaScript modules on the web  \|  Web Fundamentals  \|  Google Developers](https://developers.google.com/web/fundamentals/primers/modules)

> 拡張子は`.mjs`にする

```js
// FILE: lib.mjs
export const repeat = string => `${string} ${string}`;
export function shout(string) {
  return `${string.toUppserCase()}!`;
}

// FILE: main.mjs
import { repeat, shout } from "./lib.mjs";
repeat("hello"); //=> 'hello hello'
shout("action"); //=> 'ACTION!'
```

```js
// defaultを使うと、どんな名前でもインポートできるようになる

// lib.mjs
export default function(string) {
  return `${string.toUpperCase()}!`;
}

// main.js
import shout from "./lib.mjs";
```

## 「webpack なしで Vue.js アプリが作れる」と言っている記事を発見

[Goodbye webpack: Building Vue\.js Applications Without webpack](https://markus.oberlehner.net/blog/goodbye-webpack-building-vue-applications-without-webpack/)

> thanks to native browser support for JavaScript modules, it’s easier than ever to build powerful JavaScript applications without using any build tools at all.

[Goodbye webpack: Building Vue\.js Applications Without webpack](https://goodbye-webpack-building-vue-applications-without-webpack.netlify.com/)

[GitHub \- maoberlehner/goodbye\-webpack\-building\-vue\-applications\-without\-webpack: This is an example project for the following article: https://markus\.oberlehner\.net/blog/goodbye\-webpack\-building\-vue\-applications\-without\-webpack/](https://github.com/maoberlehner/goodbye-webpack-building-vue-applications-without-webpack)

### 自分で確認してみた

`type="module"`さえ記述すれば、別ファイルにする必要もないことがわかった

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Toolsトップ画面</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.1/css/bulma.min.css"
    />
  </head>
  <body>
    <div id="app"></div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script type="module">
      import App from "/js/App.js";
      new Vue({
        render: h => h(App)
      }).$mount("#app");
    </script>
  </body>
</html>
```

/js/App.js

```js
export default {
  name: "App",
  data: function() {
    return {
      message: "Hello, World!"
    };
  },
  template: `
        <section class="section">
            <div class="container">
                <div class="is-size-1">
                    {{ message }}
                </div>
            </div>
        </section>
    `
};
```
