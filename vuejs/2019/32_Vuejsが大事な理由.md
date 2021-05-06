# Vue.js の概要

作成日 2019/04/15

## 01. Vue.js が大事な理由

Vue.js は、JS フレームワークである。これを利用することで**SPA（シングルページアプリケーション）**の開発が実現できることとなった。公式サイトのガイドはよく出来ているので、これをしっかり何度も繰り返し読み、頭に叩き込むべきである。Vue.js がそれほどに大事な理由を以下に述べる

[はじめに — Vue\.js](https://jp.vuejs.org/v2/guide/)

### クライアントサイドの開発は何が難しいのか

UI（ユーザーインターフェース）がないサーバーサイドの開発は、ぶっちゃけて言うと「リクエストがあったらレスポンスを返す」だけのことである。リクエストをもらってからレスポンスを返すまでにやるべきことが多いと、長たらしいコードになって後からメンテナンスしにくくなるが、最初から最後まで一本道であることに変わりはない。とくにサーバーサイドのテンプレートを廃止して、「Web サーバー = API サーバー」となってしまった今は、単純明解となっている

![vuejs-api.png](https://imgur.com/N5285pG.png)

かたや、クライアントサイドは UI を管理しなければならない。広いページに散らばっている沢山のボタンが押された時のそれぞれの処理とその処理の結果としての UI の変更を管理しなければならない。出発点のボタンすらも UI のひとつとして、登場したり消えたりするかもしれないし、同じボタンでも、ログインしているユーザーが違うと処理が変わってくることもある。SPA になると、さらにこの矢印の流れが増え、混乱の極みとなる。この混乱を整理整頓するためには、フレームワークが必要になってくる

![vuejs-ui.png](https://imgur.com/CjnWSRB.png)

### 状態(state)という概念の導入

状態(state)は、ホワイトボードみたいなものである。UI とはまったく関係ないところに、ホワイトボードを置いておき、「ユーザーはログインしているかしていないか」、「テーブルに表示すべきデータは何か」、「メッセージは今表示すべきか否か」、「もし表示すべきならそのメッセージの内容は何か」など、なんでもかんでも、あらかじめ、そこに書き込んでおく。もちろん初期値は空白の文字列やゼロで構わない

ボタンをクリックすると処理が始まるが、その処理は、状態(state)を更新するところで終わる。UI の更新についてはなにもしない。そのホワイトボードを UI のパーツたちは遠くから監視していて、何か変更があったら、あらかじめ決められたルールに基づいて自分たちで勝手に姿形を変えていく

![vuejs-state.png](https://imgur.com/gGeXjk4.png)

### 結局、何を達成できたのか

SOLID 原則のひとつである「OCP: Open-Closed Princple オープンクローズドの原則」を達成できている。この原則は「モジュールは拡張に対して開いて (Open) おり、修正に対して閉じて (Closed) いなければならない」というもので、**機能追加をする際に、既存コードを修正する必要がまったくない状態**を意味している。

既存コードをいじらずに、コードを追加していくだけで機能追加が達成できるならば、「改修」が全く怖くなくなる。むしろプログラミングが楽しくて仕方なくなる。

## 02. Vue コンポーネント

![vuejs-vue-diagram.png](https://imgur.com/pj3ny5E.png)

## 03. Vuex

Vuex は、状態(state)を管理するツールで、ストア(store)と呼ばれる。以下の 3 つのパートから成る

- state ... 状態(state)を表す JS オブジェクト
- mutations ... state を更新できるメソッド
- actions ... 非同期処理が許されるメソッド。state の更新は mutations に頼む

![vuejs-vuex.png](https://imgur.com/kbJlg2g.png)

一方向にだけ動かすことで、多数の処理の整理整頓される

## 04. ファイル構成図

```text
--client_side/
    `--kaigi/
        |--components/ ... 子コンポーネント群を置く
        |--modules/ ... モジュール群を置く
        |--pages/ ... ページコンポーネントを置く
        |--index.html ... 入口かつ親コンポーネントのテンプレート
        `--index.js ... 親コンポーネント
```

### index.js の例

```js
Vue.use(Vuex); // Vuexを使う準備
Vue.use(VueRouter); // Vue Routerを使う準備

// Vuexは複数のモジュールから成り立つ
const store = new Vuex.Store({
  modules: {
    moduleA,
    moduleB,
    moduleC
  }
});

// Vue Routerにページコンポーネントを登録する
const router = new VueRouter({
  routes: [
    { path: "/page1", component: Page1 },
    { path: "/page2", component: Page2 },
    { path: "/page3", component: Page3 }
  ]
});

// 親コンポーネントに、Vue Router・Vuex・子コンポーネントを登録する
// index.htmlの<div id="app"></div>は、親コンポーネントのテンプレートとなる
const app = new Vue({
  el: "#app",
  router,
  store,
  components: {
    "child-component-1": childComponent1,
    "child-component-2": childComponent2,
    "child-component-3": childComponent3
  }
});
```

## 05. 補足

Vue コンポーネントは、どこからでも、`this.$router`で Vue Router を呼べる

Vue コンポーネントは、どこからでも、`this.$store`で Vuex を呼べる

Vuex も、それが分割されたモジュールも、Vue コンポーネントではないので、this は使えない。

actions は、引数に context があって、それが Vuex に相当する
