# [Vuex モジュール](https://vuex.vuejs.org/ja/guide/modules.html)

作成日 2019/02/19

## ストアをモジュールに分割する

```js
const moduleA = {
    namespaced: true,
    state: { ... },
    mutations: { ... },
    actions: { ... },
    getters: { ... }
};

const moduleB = {
    namespaced: true,
    state: { ... },
    mutations: { ... },
    actions: { ... },
    getters: { ... }
};

const store = new Vuex.store({
    modules: {
        a: moduleA,
        b: moduleB
    }
});

store.state.a //=> moduleAのステート
store.state.b //=> moduleBのステート
```

### モジュールに分割後、どうやって動作を指示するか

dispatch や commit を使う場合は、path に module 名も含める必要がある。

各 module に`namespaced: true`を入れておく必要あり

```js
this.$store.commit("newConf/replacePhones", phoneListUniq.join(","));
this.$store.commit("newConf/replaceName", this.name);
```

[vuex で module を分ける方法と注意点 \- Qiita](https://qiita.com/kuriya/items/bc9a070119f0f4bfe944)
