# Composition API を勉強する

作成日 2021/09/19、更新日 2021/09/23

## 01. 公式ドキュメントを読む

公式ドキュメント => [なぜ Composition API なのか？ \| Vue\.js](https://v3.ja.vuejs.org/guide/composition-api-introduction.html#%E3%81%AA%E3%81%9B%E3%82%99-composition-api-%E3%81%AA%E3%81%AE%E3%81%8B)

### 自分なりに要約する

Vue コンポーネントも肥大化する。違う関心事が入れ子になって、読みづらく、理解しづらいものになってくる。 同じ関心事のコードだけをまとめるために、Composition API を用意した

- Vue コンポーネントに、`setup` という新しいオプションができた
- `setup` の中には、`this` が登場しない。したがって、別ファイルにすることが可能
- 型推論が改善され、TypeScript との親和性が高くなった

## 02. サンプルコードを書いてみる

- App.vue と Counter.vue で、ひとつのコードを使いまわす
- Web サーバーを起動すると、カウンターが 2 個登場する
- カウンターは連動することなく、それぞれ別個に動く

```text
--src/
    |--components/
    |   `--Counter.vue
    |--composables/
    |   `--useCount.ts
    `--App.vue
```

src/components/Counter.vue

```html
<template>
  <button @click="decrement">-</button>
  <span v-text="count" />
  <button @click="increment">+</button>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import { useCount } from '../composables/useCount';

  export default defineComponent({
    setup() {
      return {
        ...useCount(),
      };
    },
  });
</script>
```

src/composables/useCount.ts

```javascript
import { ref } from 'vue';

export const useCount = () => {
  const count = ref(0);
  const increment = () => {
    count.value += 1;
  };
  const decrement = () => {
    count.value -= 1;
  };

  return {
    count,
    increment,
    decrement,
  };
};
```

App.vue

```html
<template>
  <button @click="decrement">-</button>
  <span v-text="count" />
  <button @click="increment">+</button>
  <counter></counter>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import Counter from './components/Counter.vue';
  import { useCount } from './composables/useCount';

  export default defineComponent({
    components: { Counter },

    setup() {
      return {
        ...useCount(),
      };
    },
  });
</script>
```

### 自分なりに学んだこと

- Composition API は、状態管理を担当しているわけではない。別のコンポーネントが同じコードを使いまわしても、それぞれ別個に動く
- Vue コンポーネントにおける `setup()` は `data()` の拡張版みたいなものであり、`data()` が扱っているリアクティブな値は、`ref()` で実現している
