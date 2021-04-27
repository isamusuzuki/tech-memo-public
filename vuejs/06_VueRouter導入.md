# Vue Router 導入

作成日 2021/04/19

## Vue Router とは

公式サイト => [https://router.vuejs.org/ja/](https://router.vuejs.org/ja/)

> Vue Router は Vue.js 公式ルータです。これは Vue.js のコアと深く深く統合されており、Vue.js でシングルページアプリケーションを構築します。

## 「インストール」を読む

[インストール \| Vue Router](https://router.vuejs.org/ja/installation.html#vue-cli)

```bash
npm install vue-router
```

```javascript
import Vue from 'vue'
import VueRtouer from 'vue-router'
Vue.use(VueRouter)
```

## 「はじめに」を読む

[はじめに \| Vue Router](https://router.vuejs.org/ja/guide/#html)

HTML

```html
<div id="app">
    <p>
        <router-link to="/foo">Go to Foo</router-link>
        <router-link to="/bar">Go to Bar</router-link>
    </p>
    <router-view></router-view>
</div>
```

JavaScript

```javascript
const Foo = { template: '<div>foo</div>'}
const Bar = { template: '<div>bar</div>'}

const routes = {
    { path: '/foo', component: Foo},
    { path: '/bar', component: Bar}
}

const router = new VueRouter({
    routes
})

const app = new Vue({
    router
}).$mount('#app')
```
