# [Vue Router](https://router.vuejs.org/ja/)を復習する

作成日 2019/01/10、更新日 2019/02/15

## 確認できたこと

- コンパイルなしの CDN で、Vue Router を動かすことができた
- ページに相当する子コンポーネントを、別ファイルにすることができた
- ページに相当する子コンポーネントで、axios を使った非同期通信ができた
- 子コンポーネントのメソッドで、強制的にページ遷移させることができた
- 子コンポーネントにデータを与えるにはどうすればいいか？ => Vuex を採用して解決した
- 子コンポーネントから、親コンポーネントのメソッドを呼ぶにはどうすればいいか？ => Vuex を採用して解決した
- ページ遷移前に毎回コードを走らせるにはどうすればいいか？ => ナビゲーションガードとルートメタフィールドを使った

## 子コンポーネントの props にデータを与えるには

[ルートコンポーネントにプロパティを渡す \| Vue Router](https://router.vuejs.org/ja/guide/essentials/passing-props.html)

routes オプションで指定する location オブジェクトは、`path`, `component`, `props` の 3 つのプロパティを持つ

```js
// FILE: templates/index.phtml
const router = new VueRouter({
  routes: [{ path: "/", component: Top, props: { name: "Special Top" } }]
});

// FILE: public/js/Top.js
export default {
  props: ["name"],
  template: `
        <section class="section">
            <div class="container">
                <div class="is-size-1">
                    {{ name }}
                </div>
            </div>
        </section>
    `
};
```

残念ながら、props プロパティは、`this.$router.push(location)`では効かない

動的に子コンポーネントに与えることはできず、ルート設定するときに固まってしまうようだ

## 子コンポーネントのメソッドで、ページ遷移させるには

[プログラムによるナビゲーション \| Vue Router](https://router.vuejs.org/ja/guide/essentials/navigation.html)

> Vue インスタンスの内部では、$router としてルーターインスタンスにアクセスできます。従って、this.$router.push で呼ぶことができます。

```js
export default {
  methods: {
    jump: function() {
      this.$router.push({ path: "/" });
    }
  },
  template: `
        <button class="button is-link" v-on:click="jump">ジャンプ</button>
    `
};
```

## CDN で vue-router を実際に動かしたコード

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Banana</title>
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.7.2/css/bulma.min.css"
    />
  </head>
  <body>
    <div id="app">
      <nav class="navbar is-primary">
        <div class="navbar-brand">
          <router-link to="/" class="navbar-item">BANANA</router-link>
          <div class="navbar-burger" data-target="navMenu">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
        <div class="navbar-menu" id="navMenu">
          <router-link to="/about" class="navbar-item">About</router-link>
          <router-link to="/dashboard" class="navbar-item"
            >Dashboard</router-link
          >
        </div>
      </nav>
      <router-view></router-view>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <script type="module">
      import About from "/js/About.js";
      import Dashboard from "/js/Dashboard.js";
      import Top from "/js/Top.js";

      const router = new VueRouter({
        routes: [
          { path: "/about", component: About },
          { path: "/dashboard", component: Dashboard },
          { path: "/", component: Top }
        ]
      });

      const app = new Vue({
        el: "#app",
        router
      });
    </script>
  </body>
</html>
```

## [ナビゲーションガード](https://router.vuejs.org/ja/guide/advanced/navigation-guards.html)と[ルートメタフィールド](https://router.vuejs.org/ja/guide/advanced/meta.html)とは

ナビゲーションガードは、リダイレクトもしくはキャンセルによって遷移をガードする機能

とりあえず、グローバルガードである`router.beforeEach`の使い方だけをマスターすればよい

ルートオブジェクトは、meta フィールドを含めることができるので、それでログインが必要か不要かを判断できるようにする

```js
const router = new VueRouter({
  routes: [
    {
      path: "/foo",
      component: Foo,
      children: [
        {
          path: "bar",
          component: Bar,
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
});

router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    if (!auth.loggedIn()) {
      next({
        path: "/login",
        query: { redirect: to.fullPath }
      });
    } else {
      next();
    }
  } else {
    next();
  }
});
```

ガード関数は 3 つの引数を受け取る、常に next 関数を呼び出すこと

- to ... 次に遷移される対象のルートオブジェクト
- from ... 遷移される前の現在のルートオブジェクト
- next() ... パイプラインの次のフックに移動する
- next({path: '/'}) ... 現在の遷移を中止し、新しい遷移を始める
- \$route.matched ... ルートにマッチした全てのルートレコードの配列
- Array.prototype.some() ... 配列の少なくとも 1 つの要素が、渡されたテストを通れば true を返す

## [動的ルートマッチング](https://router.vuejs.org/ja/guide/essentials/dynamic-matching.html)を勉強する

パターンを使って同じコンポーネントにルートをマップする必要がしばしばあるはず

例えば`StatusPage`コンポーネントは、すべてのカンファレンスに対して描画されるべきであるが、
それぞれ異なる ID を持つ

vue-router は、パスの中の動的セグメントを使用して実現できる

```js
export default {
  data: function() {
    return {
      id: 0
    };
  },
  mounted: function() {
    this.id = this.$route.params.id;
  },
  watch: {
    "$route.params": function() {
      this.id = this.$route.params.id;
    }
  }
};

const router = new VueRouter({
  routes: [{ path: "/status/:id", compoonent: StatusPage }]
});
```

動的セグメントはコロン`:`を使って表し、これは全てのコンポーネント内で`this.$route.params`として利用可能になる

mounted にしか`this.$route.params`から id を引っ張ってくるコードがないと、
ページ表示した後に URL だけ変更した場合に変更されない => watch もつける
