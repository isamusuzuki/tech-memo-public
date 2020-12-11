# HTML テンプレート

作成日 2019/04/15、更新日 2019/04/19

![html-template-screenshot.png](https://imgur.com/40wBqjV.png)

## 01. CDN を使うとき用

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <title>HTML Template</title>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.4/css/bulma.min.css"
    />
    <link
      rel="stylesheet"
      href="https://use.fontawesome.com/releases/v5.8.1/css/all.css"
      integrity="sha384-50oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf"
      crossorigin="anonymous"
    />
  </head>
  <body>
    <div id="app">
      <section class="section">
        <div class="container">
          <div class="title">
            <span class="icon has-text-info">
              <i class="fas fa-hand-spock"></i>
            </span>
            {{ message }}
          </div>
        </div>
      </section>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script>
      const app = new Vue({
        el: '#app',
        data: {
          message: 'Hello, World!',
        },
      });
    </script>
  </body>
</html>
```

## 02. Parcel でバンドルするとき用

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
import Vue from 'vue';
import App from './App';

new Vue({
  el: '#app',
  template: '<App/>',
  components: { App },
});
```

App.vue

```html
<template>
  <section class="section">
    <div class="container">
      <div class="title">
        <span class="icon has-text-info">
          <i class="fas fa-hand-spock"></i>
        </span>
        {{ message }}
      </div>
    </div>
  </section>
</template>

<script>
  import Vue from 'vue';
  import Meta from 'vue-meta';
  Vue.use(Meta);
  export default {
    metaInfo: {
      title: 'HTML Template',
      htmlAttrs: {
        lang: 'ja',
      },
      meta: [{ charset: 'utf-8' }],
      link: [
        {
          rel: 'stylesheet',
          href: 'https://use.fontawesome.com/releases/v5.8.1/css/all.css',
          integrity:
            'sha384-50oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf',
          crossorigin: 'anonymous',
        },
      ],
    },
    data() {
      return {
        message: 'Hello, World!',
      };
    },
  };
</script>

<style>
  @import '../node_modules/bulma/css/bulma.min.css';
</style>
```
