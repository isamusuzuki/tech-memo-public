# Vue Router 導入

作成日 2021/04/19、更新日 2021/05/03

## 01. Vue Router とは

公式サイト => [https://router.vuejs.org/ja/](https://router.vuejs.org/ja/)

> Vue Router は Vue.js 公式ルータです。これは Vue.js のコアと深く深く統合されており、Vue.js でシングルページアプリケーションを構築します。

### 「インストール」を読む

[インストール \| Vue Router](https://router.vuejs.org/ja/installation.html#vue-cli)

```bash
npm install vue-router
```

```javascript
import Vue from 'vue'
import VueRtouer from 'vue-router'
Vue.use(VueRouter)
```

### 「はじめに」を読む

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

## 02. ページコンポーネントに引数を与える

参考記事 => [ルートコンポーネントにプロパティを渡す \| Vue Router](https://router.vuejs.org/ja/guide/essentials/passing-props.html)

```javascript
const User = {
    template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
    routes: [
        { path: '/user/:id', component: User},
    ]
})
```

- ルートのパラメーターは string 型として扱われる。`$route.params` 自体が `Dictionary<string>` と定義されている
- 引数ありのリンクを設定するときは、テンプレートに `<router-link></router-link>` を書き込むよりも、メソッドの中で `router.push()` と名前付きルートを使うのがベター

名前付きルートの参考記事 => [名前付きルート \| Vue Router](https://router.vuejs.org/ja/guide/essentials/named-routes.html)

> しばしば、名前を使ってルートを特定できるとより便利です。特にルートにリンクするときやナビゲーションを実行するときなどです。Router インスタンスを作成するときに routes オプションの中でルートに名前を付けることができます。

```javascript
const router = new VueRouter({
  routes: [
    { path: '/user/:userId', name: 'user', component: User }
  ]
})

export default Vue.extend({
    router,
    methods: {
        goUser() {
            const idStr = String(this.item.id)
            this.$router.push({name: 'user', params: {userId: idStr}})
        }
    }
})
```
