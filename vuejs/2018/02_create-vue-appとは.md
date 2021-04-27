# create-vue-app とは

作成日 2018/04/27

## create-vue-app を使う

`yarn create`コマンドでアプリケーションを新規作成する

リポジトリ => [vue\-land/create\-vue\-app: Create Vue apps with no build configuration\.](https://github.com/vue-land/create-vue-app)

使い方 => `yarn create vue-app {project}`

出される質問とその返答例

```text
? What's the name of your new project => 04create
? How Would your describe your superb project => My rad Vue project
? Add Progressive Web App (PWA) support => No
? Use Tyu(Jest) to run unit tests => No
? What's your GitHub username => Isamu Suzuki
? What's your GitHub email => isamusuzuki@outlook.com
```

## 新規プロジェクトを作成してみてわかったこと

- [poi](https://poi.js.org/)を使っている
- HTML テンプレートが ejs 形式になっている
- package.json に書き込まれている依存関係がものすごく少ない、3 個＋ 3 個
- Webpack も Vue.js も隠れてしまっている => 実際は使っている
- Webpack の設定が不要 => とてもありがたい
- vue-router が入っていないなど、作者である[egoist](https://github.com/egoist)の趣向が強い

## このプロジェクトに vue-router を組み込む

- インストール `yarn add vue-router`
- `src/index.js`で、Vue インスタンスに自作した router を組み込む
- `src/router/index.js`で、Router インスタンスをエクスポートする
- `src/components/App.vue`で、`<router-link>`と`<router-view>`を配置する
- `src/components/Top.vue`他で、各ページを生成する

```javascript
// src/index.js
import Vue from 'vue';
import App from './components/App.vue';
import router from './router';
new Vue({
  el: '#app',
  router,
  render: (h) => h(App),
});

// src/router/index.js
import Vue from 'vue';
import Router from 'vue-router';
import Top from '@/components/Top';
import Signup from '@/components/Signup';
import Signin from '@/components/Signin';
Vue.use(Router);
export default new Router({
  base: '/',
  routes: [
    { path: '/', name: 'Top', component: Top },
    { path: '/signup', name: 'Signup', component: Signup },
    { path: '/signin', name: 'Signin', component: Signin },
    { path: '*', redirect: '/' },
  ],
});
```

```html
<!-- src/components/App.vue -->
<template>
  <div id="app">
    <div class="menu">
      <router-link to="/" exaxt>トップ</router-link>
      <router-link to="/signup">Signup</router-link>
      <router-link to="/signin">Signin</router-link>
    </div>
    <div class="main">
      <router-view />
    </div>
  </div>
</template>

<!-- src/components/Top.vue -->
<template>
  <h1>{{name}}</h1>
</template>
<script>
  export default {
    data() {
      return {
        name: 'Top',
      };
    },
  };
</script>
```

## このプロジェクトに Bulma を組み込む

- インストール `yarn add bulma`
- `src/components/App.vue`で css ファイルをインポートする
- モバイルのときに、メニューを隠すスクリプトは、created メソッドに書き込む
- メニューを選択してもドロップダウンが開いたままになる問題がある

```html
<style lang="css">
  @import '../../node_modules/bulma/css/bulma.css';
</style>
```

## このプロジェクトに Firebase を組み込む

2017/12/25 の記事：[Vue\.js \+ Firebase を使って爆速でユーザ認証を実装する \- Qiita](https://qiita.com/sin_tanaka/items/ea149a33bd9e4b388241)

### サーバーサイドの設定

[Forebase console](https://console.firebase.google.com/)に行き、FriendlyChat をクリック

右上にある「別のアプリを追加」をクリック ＞ 「ウェブアプリに Firebase を追加」をクリック

スニペットを取得する

```html
<script src="https://www.gstatic.com/firebasejs/4.10.0/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: 'AIzaSyAF3_OJSm5137RukFsRwkBEMKM5qOuG0bE',
    authDomain: 'friendlychat-770e0.firebaseapp.com',
    databaseURL: 'https://friendlychat-770e0.firebaseio.com',
    projectId: 'friendlychat-770e0',
    storageBucket: 'friendlychat-770e0.appspot.com',
    messagingSenderId: '1070426866033',
  };
  firebase.initializeApp(config);
</script>
```

### Vue.js アプリでの設定

- インストール `yarn add firebase`
- `src/index.js`で、プロジェクト固有のプロパティを組み込む

```js
// src/index.js
import firebase from 'firebase';

const config = {
  apiKey: 'AIzaSyAF3_OJSm5137RukFsRwkBEMKM5qOuG0bE',
  authDomain: 'friendlychat-770e0.firebaseapp.com',
  databaseURL: 'https://friendlychat-770e0.firebaseio.com',
  projectId: 'friendlychat-770e0',
  storageBucket: 'friendlychat-770e0.appspot.com',
  messagingSenderId: '1070426866033',
};
firebase.initializeApp(config);
```
