# [Vuex](https://vuex.vuejs.org/ja/)を復習する

作成日 2019/01/11、更新日 2019/02/22

## 一番簡単なサンプルコード

1. Vuex ストアを作成する
1. state（ストアオブジェクトの定義）と mutations（ストアオブジェクトの変更）を定義する
1. ストアオブジェクトの状態を参照したいときは、`store.state`
1. ストアオブジェクトの状態を変更したいときは、`store.commit(mutation)`
1. Vuex ストアを Vue コンポーネントに入れる
1. Vuex ストアは、すべてのコンポーネントで、`this.$store`で参照できる

```html
<span>{{ count }}</span>&nbsp;&nbsp;
<button class="button is-info" v-on:click="increment">Click</button>
<script src="https://unpkg.com/vuex/dist/vuex.js"></script>
<script>
  const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      increment(state) {
        state.count++;
      }
    }
  });

  const app = new Vue({
    el: "#app",
    store,
    computed: {
      count() {
        store.state.count;
      }
    },
    methods: {
      increment: function() {
        store.commit("increment");
      }
    }
  });
</script>
```

## Vuex ストアが持つオプション

- state ... 状態の保持
- mutations ... 実際に状態の変更を行う、非同期処理 NG
- getters ... ストアの算出プロパティ
- actions ... 状態を変更しない、ミューテーションをコミットするだけ、非同期処理 OK

## Vuex ミューテーションに関して、とても大事なこと

[ミューテーション \| Vuex](https://vuex.vuejs.org/ja/guide/mutations.html)

> `store.commit`に追加の引数を渡すこともできます。この追加の引数は、特定のミューテーションに対するペイロードと呼びます。ほとんどの場合、ペイロードはオブジェクトにすべきです。そうすることで複数のフィールドを含められるようになり、またミューテーションがより記述的に記録されるようになります:

これは「複数のフィールドがある場合は、ペイロードをオブジェクトにすることで対処しなさい、間違っても第三引数を使ってはいけないよ」ということを言っている。

結論：mutations は actions と同様に、追加の引数は、ペイロードの中に納めないといけない
