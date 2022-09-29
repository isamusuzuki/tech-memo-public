# Vuex 4 を導入する

作成日 2021/09/20

## 01. Vuex 4 をインストールする

```bash
npm i -D vuex@next
```

[インストール \| Vuex](https://next.vuex.vuejs.org/ja/installation.html)

## 02. TypeScript をサポートする

公式ドキュメントを写経する

[TypeScript サポート \| Vuex](https://next.vuex.vuejs.org/ja/guide/typescript-support.html)

```text
--src/
    |--App.vue    ... 型付けされたストアを取得する
    |--index.ts   ... storeとkeyをVueAppインスタンスに渡す
    |--store.ts   ... useStore 関数の型付け
    `--vuex.d.ts  ... $store プロパティの型付け
```

### 02a. Vue コンポーネントでの $store プロパティの型付け

Vuex は、すぐに使用できる`this.$store` プロパティの型付けを提供していないので、独自のモジュール拡張を宣言する。ComponentCustomProperties のカスタム型付けを宣言する

src/vuex.d.ts

```javascript
import { ComponentCustomProperties } from 'vue'
import { Store } from 'vuex'

declare module '@vue/runtime-core' {
  // ストアのステートを宣言する
  interface State {
    count: number
  }

  // `this.$store` の型付けを提供する
  interface ComponentCustomProperties {
    $store: Store<State>
  }
}
```

### 02b. useStore 関数の型付け

src/store.ts

```javascript
import { InjectionKey } from 'vue';
import { createStore, Store } from 'vuex';

// ストアのステートに対して型を定義する
export interface State {
  count: number;
}

// インジェクションキーを定義する
export const key: InjectionKey<Store<State>> = Symbol();

// ストアを生成する
export const store =
  createStore <
  State >
  {
    state: {
      count: 0,
    },
  };
```

src/index.ts

```javascript
import { createApp } from 'vue'
import { store, key } from './store'

const app = createApp({ ... })

// キーをVue Appインスタンスに渡す
app.use(store, key)

app.mount('#app')
```

### 02c. キーを useStore メソッドに渡す

App.vue

```javascript
import { useStore } from 'vuex';
import { key } from './store';

export default {
  setup() {
    const store = useStore(key);

    store.state.count; // typed as number
  },
};
```

### 02d. 写経してみて気づいたこと

- コンポーネントの `methods` では、ストアをいじらないならば、`src/vuex.d.ts` ファイルは不要
- Composition API でしか、ストアを使わなければいいだけのこと。これなら守れそうだ
