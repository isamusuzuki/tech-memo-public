# コンポーネントをrefなどのリアクティブ変数に格納する

作成日 2025/09/25

## AIによる概要

Vue.jsでコンポーネントをリアクティブ変数に格納するには、`<script setup>`内でref()またはreactive()関数を使ってコンポーネントインスタンスをラップし、リアクティブなオブジェクトを作成します。その後、そのリアクティブ変数をテンプレート内で利用することで、データの変更に連動したUIの自動更新が実現されます。

### ref()を使用する

```html
<script setup>
import { ref } from 'vue';

// ref()でコンポーネントインスタンスをリアクティブにする
const myComponent = ref(null); // 初期値はnullなど
</script>

<template>
  <MyComponent ref="myComponent" />
  <!-- myComponentはリアクティブなので、内容が変わるとUIも更新される -->
</template>
```

- `ref()`でコンポーネントに参照を割り当てます。
- `<template>`内でrefに指定した名前（例: myComponent）でアクセスできます。
- myComponentが指すコンポーネントの内容が変更された場合、Vueはそれを検知しUIを更新します。

### reactive()を使用する

```html
<script setup>
import { reactive } from 'vue';

const state = reactive({
  user: {
    name: 'Alice',
    age: 30
  }
});

// コンポーネントから受け取ったデータをstateに格納する
// <MyChildComponent :user-data="state.user" />
</script>

<template>
  <!-- state.userの内容が変更されると、UIが自動更新される -->
  <div>{{ state.user.name }} - {{ state.user.age }}</div>
</template>
```

- `reactive()`でオブジェクトを作成し、コンポーネントのデータとして利用します。
- state.user.nameのようにオブジェクト内のプロパティを変更するだけで、UIは自動的に更新されます。
