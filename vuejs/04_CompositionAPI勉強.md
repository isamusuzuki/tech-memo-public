# Composition API を勉強する

作成日 2021/09/19

## 01. 公式ドキュメントを読む

[なぜ Composition API なのか？ \| Vue\.js](https://v3.ja.vuejs.org/guide/composition-api-introduction.html#%E3%81%AA%E3%81%9B%E3%82%99-composition-api-%E3%81%AA%E3%81%AE%E3%81%8B)

### 公式ドキュメントを自分なりに要約する

Vue コンポーネントも大きくなってくると、論理的な関心事が別のコードが入れ子になってきて、読みづらく、理解しづらいものになってくる。 同じ論理的な関心事に関連するコードだけをまとめるために、Composition API を用意した

- Vue コンポーネントに、`setup` という新しいオプションができた
- `setup`の中には、`this` が登場しない。したがって、別ファイルにすることが可能

## 02. サンプルコードを書いてみる

- App.vue と Counter.vue で、ひとつのコードを使いまわす
- Web サーバーを起動すると、カウンターが 2 個登場する
- カウンターは連動することなく、それぞれ別個に動く

```text
--src/
    |--components/
    |   `--Counter.vue
    |--hooks/
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
  import { useCount } from '../hooks/useCount';

  export default defineComponent({
    setup() {
      return {
        ...useCount(),
      };
    },
  });
</script>
```

src/hooks/useCount.ts

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
  import { useCount } from './hooks/useCount';

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

- Composition API は、関心の分離を達成するために生まれた
- `setup()` メソッドの中では、`this`が消えたことでそっくり別のファイルに切り出すことが可能になった
- Composition API は、状態管理を担当しているわけではない。別のコンポーネントが同じコードを使いまわしても、それぞれ別個に動く
