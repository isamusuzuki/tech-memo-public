# Vuex の配置と操作

作成日 2019/02/22

機能を追加しようと思ったときに、コード（ファイル）を増やすことで機能が追加され、親コンポーネントを除く既存のコードをいじることがない

まさに、OCP の原則（追加にオープンで、改修にクローズ）が実現できた

## 具体的な例

非同期のアクセスをしているときにローディング中のアイコンを表示させたい

```text
--/js/kaigi/
    |--components/
    |   `--loadingModal.js
    |--modules/
    |   `--loader.js
    `--pages/
        `--home.js
--index.html
```

## コンポーネントの役割

ストアの state（状態）をディレクティブを使って HTML にレンダリングさせること

```js
export default {
  computed: {
    isActive() {
      return this.$store.state.loader.isActive;
    }
  },
  template: `
        <div class="modal" v-bind:class="{'is-active': isActive}">
        </div>
    `
};
```

## モジュールの役割

- ストアの state（状態）
- ストアの state（状態）を唯一操作できる mutations
- 非同期な操作ができる actions

```js
export default {
  namespaced: true,
  state: {
    isActive: false
  },
  mutations: {
    update(state, isActive) {
      state.isActive = isActive;
    }
  },
  actions: {
    on(context) {
      return new Promise((resolve, reject) => {
        context.commit("update", true);
        resolve();
      });
    },
    off(context) {
      return new Promise((resolve, reject) => {
        context.commit("update", false);
        resolve();
      });
    }
  }
};
```

## 操作するときの作法

actions はすべて Promise を返すので、then メソッドでチェーンを構成できる

```js
export default {
  methods: {
    spin() {
      this.$store.dispatch("loader/on").then(() => {
        setTimeout(() => {
          this.$store.dispatch("loader/off");
        }, 3000);
      });
    }
  }
};
```
