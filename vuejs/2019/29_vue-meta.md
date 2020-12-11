# vue-meta を導入する

作成日 2019/04/12

ページのメタ情報を Vue コンポーネントで管理するツール

公式サイト => [https://github.com/nuxt/vue-meta](https://github.com/nuxt/vue-meta)

インストール => `npm install vue-meta --save`

App.vue

```html
<template>
  <div></div>
</template>

<script>
  import Vue from "vue";
  import Meta from "vue-meta";

  Vue.user(Meta);

  export default {
    metaInfo: {
      title: "郵便番号検索",
      htmlAttrs: {
        lang: "ja"
      },
      meta: [{ charset: "utf-8" }],
      link: [
        {
          rel: "stylesheet",
          href: "https://use.fontawesome.com/releases/v5.8.1/css/all.css",
          integrity:
            "sha384-50oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf",
          crossorigin: "anonymous"
        }
      ]
    }
  };
</script>
```

## vue-meta を採用して何が嬉しいのか

Parcel でバンドルするための起点となっている、index.html と index.js が
まったくいじる必要がなくなり、固定化されたことが嬉しい

index.html

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="app"></div>
    <script src="index.js"></script>
  </body>
</html>
```

index.js

```js
import Vue from "vue";
import App from "./App";

new Vue({
  el: "#app",
  template: "<App/>",
  components: { App }
});
```
