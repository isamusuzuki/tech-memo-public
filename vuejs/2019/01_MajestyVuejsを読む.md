# "The Majesty of Vue.js 2"を読む

作成日 2017/10/06、更新日 2017/10/09

## I. Vue.js Fundamentals

### 2. Getting Started

el オプション ... エレメントを指定する

data オプション ... 変数の初期値を与える

```js
new Vue({
  el: "#app",
  data: {
    message: "Greetings your majesty!"
  }
});
```

v-model ディレクティブ ... 双方向データバインディングを実現する

```html
<div id="app">
  <h1>{{ message }}</h1>
  <input v-model="mesage" />
</div>
```

### 3. Flavor of Directives

v-show ディレクティブ ... 条件を満たせばエレメントを表示する、反対時は`display:none`

v-if ディレクティブ ... 条件を満たせば、エレメントを表示する、反対時は要素取り去り

v-else ディレクティブ ... v-if と対になって使用できる

```html
<h1 v-if="!message">You must send a message for help!</h1>
<h2 v-else>You have sent a message</h2>
<button v-show="message">Send word to allies for help!</button>
```

### 4. List Rendering

v-for ディレクティブ ... 配列の中のアイテムを使って繰り返す

```html
<ul class="list-group">
  <li v-for="(story, index) in stories" class="list-group-item">
    {{index + 1}} {{story.writer}} said "{{story.plot}}".
  </li>
</ul>
```

### 5. Interactivity

v-on ディレクティブ ... 要素にイベントリスナーを取り付ける。イベントタイプは引数で指定

```html
<button v-on:click="up">Upvote: {{upvotes}}</button>

<script>
  new Vue({
    el: ".container",
    data: {
      upvotes: 0
    },
    methods: {
      up: function() {
        this.upvotes++;
      }
    }
  });
</script>
```

`v-on:click`は、`@click`と置き換えてもオーケー

```html
<button @click="up">Upvote: {{upvotes}}</button>
<button @click="down">down</button>
```

`v-on`には、4 つのイベントモディファイアが用意されている `.prevent` `.stop` `.capture` `.self`

`.prevent`を使うと、`function(event) {event.preventDefault()}`を設定したのと同じ効果がある

```html
<button type="submit" @click.prevent="calc" class="btn btn-primary">
  Calc
</button>
```

キーボードイベントには、キーコードのアリアスが用意されている `enter` `tab` `delete` ...

```html
<input v-model.number="a" @keyup.enter="calc" class="form-control" />
<input v-model.number="b" @keyup.enter="calc" class="form-control" />
```

フォーム全体に、ページ送信を禁止させることもできる

```html
<form @submit.prevent="calc"></form>
```

computed オプション ... ありものから自動計算で得られたプロパティを提供する

```html
<div class="container">
  a ={{a}}, b={{b}}
  <input v-model.number="a" />
</div>
<script>
  new Vue({
    el: ".container",
    data: {
      a: 1
    },
    computed: {
      b: function() {
        return this.a + 3;
      }
    }
  });
</script>
```

computed オプションを使えば、イベントリスナーを用意する必要がなくなる

=> select の変更にも対応

### 6. Filters

メソッドでフィルターした例

```html
<li v-for="story in storiesBy('Alex')" class="list-group-item">
  {{story.writer}} said "{{story.plot}}"
</li>

<script>
  new Vue({
    el: ".container",
    data: {
      stories: [
        { writer: "Alex", plot: "Bathroom" },
        { writer: "John", plot: "Kitchen" },
        { writer: "Alex", plot: "Living room" },
        { writer: "John", plot: "Basement" }
      ]
    },
    methods: {
      storiesBy: function(writer) {
        return this.stories.filter(function(story) {
          return story.writer == writer;
        });
      }
    }
  });
</script>
```

カスタムフィルター機能を使う

```html
<li v-for="hero in heroes" class="list-group-item">
  {{hero | snitch}}
</li>

<script>
  Vue.filter("snitch", function(hero) {
    return hero.name + " is " + hero.realname + " in real life!";
  });

  new Vue({
    el: ".container",
    data: {
      heroes: [
        { name: "Batman", realname: "Bruce Waynn" },
        { name: "Superman", realname: "Clark Kent" },
        { name: "Flash", realname: "Jay Greetch" },
        { name: "Spider-man", realname: "Peter Parker" }
      ]
    }
  });
</script>
```

### 7. Components

コンポーネントは何回も使うことができる

```js
// <story></story>
// <story></story>
// <story></story>

Vue.component("story", {
  template: "<h1>My horse is amazing!</h1>"
});
```

props オプションを使って、コンポーネントにプロパティを与える

```js
// <sotry place="kitchen"></story>
// <sotry place="bathroom"></story>
// <sotry place="living"></story>

Vue.component("story", {
  props: ["place"],
  template: "<h1>My {{place}} is amazing!</h1>"
});
```

v-bind ディレクトリを使えば、オブジェクトをプロパティとして渡せる

```js
// <story v-bind:story="{plot: 'my horse is amazing', writer: 'Alex'}"></story>

Vue.component("story", {
  props: ["story"],
  template: '<h1>{{story.writer}} said "{{story.plot}}"</h1>'
});
```

v-bind ディレクティブは、`:`と略すことができる

### 8. Custom Events

```js
// 単純な形
new Vue({
  el: ".container",
  data: {
    votes: 0
  },
  methods: {
    vote: function() {
      this.votes++;
    }
  }
});

// わざわざカスタムイベントをくっつけて発射する
new Vue({
  el: ".container",
  data: {
    votes: 0
  },
  methods: {
    vote: function() {
      this.$emit("voted");
    }
  },
  created() {
    this.$on("voted", function() {
      this.votes++;
    });
  }
});
```

ライフサイクル・フックは、主に以下のようなものがある

- created ... インスタンスが作成された後
- mounted ... インスタンスが DOM にマウントされた後
- updated ... バーチャル DOM が再レンダーされるようなデータ変化の後

親子コンポーネント間の通信

`@outside="countVote"` ... 子供のアクションと親のメソッドを結びつけるコード

`@outside` => `v-on:outside` つまりはカスタムイベントということだ

```html
<div class="container">
  <p style="font-size: 140px;">{{votes}}</p>
  <food @outside="countVote" name="Cheeseburger"></food>
</div>

<script>
  Vue.component("food", {
    template: '<button class="btn btn-default" @click="vote">{{name}}</button>',
    props: ["name"],
    methods: {
      vote: function() {
        this.$emit("outside");
      }
    }
  });

  new Vue({
    el: ".container",
    data: {
      votes: 0
    },
    methods: {
      countVote: function() {
        this.votes++;
      }
    }
  });
</script>
```

非親子コンポーネント間の通信

bus というインスタンスがあるだけのコンポーネントをつくって、そこにメソッドを登録し、
そのメソッドを発射すれば、2 つのコンポーネントで通信が可能

```js
var bus = new Vue();

// それぞれのメソッドでbusのメソッドを発射する
bus.$emit("voted", food);
bus.$emit("reset");

// それぞれのcreated()で、busにメソッドを登録する
bus.$on("reset", this.reset);
bus.$on("voted", this.countVote);

// 単独ファイルコンポーネントの場合
// どちらでも使える必要があるので
// windowオブジェクト内に作成する
import Vue from "vue";
window.bus = window.bus ? window.bus : new Vue();
```

イベントリスナーを削除するには、`$off()`を使う

$off() ... すべてのイベントリスナーを削除する
$off([event]) ... ある特定のイベントに関するすべてのイベントリスナーを削除する
\$off([event, callback]) ... ある特定のイベントリスナーを削除する

### 9. Class and Style Bindings

要素に対する class プロパティを操作したいときは、`v-bind:class={'name': boolean}`を使う

boolean を操作することで、class プロパティには true の名前が適用される

`v-bind:class`は通常の class プロパティと両立できる

```html
<div class="box" v-bind:class="{'red':color, 'blue': !color}"></div>
<button v-on:click="flipColor" class="btn">Flip</button>

<script>
  new Vue({
    el: ".container",
    data: {
      color: true
    },
    methods: {
      flipColor: function() {
        this.color = !this.color;
      }
    }
  });
</script>
<style>
  .red {
    background: red;
  }
  .blue {
    background: blue;
  }
</style>
```

`v-bind:class=['classA', 'classB']`という配列書きも可能

インライン if といって、`v-bind:class=[ color ? 'red' : 'green' ]`という書き方も可能

## II. Consuming an API

[GitHub \- hootlex/the\-majesty\-of\-vuejs\-2: Demo APIs and homework solutions for "The Majesty of Vue\.js 2"](https://github.com/hootlex/the-majesty-of-vuejs-2)

### HTTP 通信ライブラリと組み合わせる

- 自分を vm と定義しておけば、プロパティで値を挿入できることがわかった
- axios の response.data に戻り値が入っていることがわかった

```js
import Vue from "vue";
import axios from "axios";

let vm = new Vue({
  el: "#app",
  data: {
    stories: []
  },
  template:
    '<ul><li v-for="story in stories">{{story.writer}} said "{{story.plot}}"</li></ul>',
  mounted: function() {
    axios
      .get(
        "http://robotgon-test.s3-website-ap-northeast-1.amazonaws.com/sample.json"
      )
      .then(response => {
        vm.stories = response.data;
      });
  }
});
```

## III. Building Large-scale Applications

### 14. ECMAScript 6

let 宣言、const 宣言、アロー関数、モジュール、クラス、デフォルト値、テンプレート文字列

### 15. Advanced Workflow

Babel でコンパイル、Node.js プロジェクト、Gulp でコンパイル、Webpack でコンパイル

### 16. Working with Single File Components

vue-cli をインストールする `npm install vue-cli -g`

vue-cli の文法は、`vue init <template-name> <project-name>`

`vue list`で、どんなテンプレートがあるかわかる

フルフィーチャーの webpack テンプレートを使ってプロジェクト生成する

`vue init webpack stories-classic-project`

vue-cli が出してくる質問に対して

- ビルドは Runtime+Compiler を選ぶ
- vue-router はインストールしない
- ESLint は使う
- ESLint のプリセットは standard を選ぶ
- Karma+Mocha はインストールしない
- Nightwatch はインストールしない

```bash
cd stories-classic-project
npm install
npm run dev
```

自分でコンポーネントをつくる => App.vue の components に足せばいい

ネストしたコンポーネントをつくる => import 文のファイル名は相対であることに注意して、components フォルダにあるファイル同士でインポートする

### 17. Eliminating Duplicate State

データのハードコードを避ける方法 1 => プロパティを共有する

```html
<stories v-bind:stories="stories"></stories>
<!-- v-bind:は:に省略できる -->
<stories :stories="stories"></stories>
```

プロパティは直接の親子関係にあるコンポーネント同士でないと使えない欠点がある

データのハードコードを避ける方法 2 => データは別ファイルに書く

```js
// file: src/store.js
export const store = {
  stories: [ ... ]
}

// file: src/components/Stories.vue
import {store} from '../store.js'

export default {
  data () {
    return {
      stories: store.stories
    }
  }
}
```

### 18. Swapping Components

ダイナミックコンポーネント

```html
<!-- 特別属性のis -->
<component is="HelloWorld"></component>

<!-- currentComponentプロパティを切り替える -->
<component :is="currentComponent"></component>
<a href="#" @click="currentComponent = 'HelloWorld'">Show Hello</a>
<a href="#" @click="currentComponent = 'Register'">Show Register</a>
<script>
  data () {
    return   {
      currentComponent: 'HelloWorld'
    }
  }
</script>
```

## 19. Vue Router

### Vue Router を組み込む

`npm install --save vue-router`

router フォルダを使って関心の分離を徹底する

```js
// src/main.js
import Vue from "vue";
import App from "./App.vue";
import router from "./router";

new Vue({
  el: "#app",
  router,
  template: "<App/>",
  components: { App }
});
```

```js
// src/router/index.js
import Vue from "vue";
import Router from "vue-router";
import HelloWorld from "../components/HelloWorld";
import Login from "../components/Login";

Vue.use(Router);

export default new Router({
  routes: [
    { path: "/", component: HelloWorld },
    { path: "/login", component: Login }
  ]
});
```

```html
<!-- src/App.vue -->
<template>
  <div id="app">
    <img src="./assets/logo.png" />
    <h1>Welcome to Routing!</h1>
    <router-link to="/">Home</router-link>
    <router-link to="/login">Login</router-link>
    <router-view></router-view>
  </div>
</template>
```

### ルート名を使う

将来パスの変更があると、いちいちリンクを探して更新する必要がでてくるので、
router-link エレメントにはルート名を与える

```js
// 使用前
{path: '/login', component: Login}
// 使用後
{path: '/login', name: 'login', component: Login}
```

```html
<!-- 使用前 -->
<router-link to="/login">Login</router-link>

<!-- 使用後 -->
<router-link :to="{name: 'login'}">Login</router-link>
```

### ヒストリーモードとは

ハッシュモード ... ハッシュつきのアドレス `/#/login`
ヒストリーモード ... ハッシュなしの普通の URL `/login`

Vue Router のデフォルトはハッシュモードだが、以下のように設定するとヒストリーモードになる

```js
// src/main.js
export default new Router({
  mode: "history",
  base: "/",
  routes: [
    { path: "/", name: "home", component: HelloWorld },
    { path: "/login", name: "login", component: Login }
  ]
});
```

### ルーターのネスト

```js
{
  routes: [
    { path: "/", name: "home", component: HelloWorld },
    { path: "/login", name: "login", component: Login },
    {
      path: "/stories",
      component: StoriesPage,
      children: [
        {
          path: "",
          name: "stories.all",
          component: StoriesAll
        },
        {
          path: "famous",
          name: "stories.famous",
          component: StoriesFamous
        }
      ]
    }
  ];
}
```

### スタイルの適用

スタイルに、`.router-link-active`を指定しておくと、アクティブになったときに
そのスタイルが適用される。ただしホームリンクが常に適用されてしまうので、`exact`プロパティを追加する

### ルートオブジェクト

`this.$route`でルートコンテクストオブジェクトにアクセスできる

### ダイナミックセグメント

path プロパティでコロンを使うと、パラメーターとして使えるようになる

```js
const routes = {
  {
    path: ':id/edit',
    name: 'stories.edit',
    component: StoriesEdit
  }
}
```

router-link エレメントで、パラメーターを指定する

```html
<router-link
  :to="{name: 'stories.edit', params: { id: story.id }}"
  tag="button"
  class="btn btn-default"
  exact
  >Edit</router-link
>
```

表示されたページで、パラメーターを取得する

```js
import { store } from "../store.js";

export default {
  data() {
    return {
      story: {}
    };
  },
  methods: {
    isTheOne(story) {
      return story.id === this.id;
    }
  },
  mounted() {
    this.id = Number(this.$route.params.id);
    this.story = store.stories.find(this.isTheOne);
  }
};
```

コンポーネントからルートオブジェクトを分離する

```js
const routes = [
  {
    path: ":id/edit",
    props: route => ({ id: Number(route.params.id) }),
    name: "stories.edit",
    component: StoriesEdit
  }
];
```

```js
export default {
  props: ["id"],
  data() {
    return {
      story: {}
    };
  },
  methods: {
    isTheOne(story) {
      return story.id === this.id;
    }
  },
  mounted() {
    this.story = store.stories.find(this.isTheOne);
  }
};
```

### ナビゲーション

`router.push(path)`を使ってプログラムからページ遷移を指示することができる

```js
this.$router.push("/stories");
this.$router.push({ path: "/stories/11/edit" });
this.$router.push({ name: "stories.edit", params: { id: "11" } });
this.$router.back();
this.$router.go(-1);
```

### ページ遷移にアニメーションを追加する

```html
<!-- src/App.vue -->
<template>
  <transition name="fade">
    <router-view></router-view>
  </transition>
</template>

<style>
  .fade-enter {
    opacity: 0;
  }
  .fade-enter-active {
    transition: opacity 1s;
  }
  .fade-enter-to {
    opacity: 0.8;
  }
</style>
```

### ナビゲーションガード

`router.beforeEach()`を使って遷移前にチェックできる

```js
// src/main.js
let User = { isAdmin: false };

router.beforeEach((to, from, next) => {
  if (to.path === "/" || to.path === "/login") {
    // 公開ページ
    next();
  } else {
    // 非公開ページ
    if (!User.isAdmin) {
      next("/login");
    } else {
      next();
    }
  }
});
```
