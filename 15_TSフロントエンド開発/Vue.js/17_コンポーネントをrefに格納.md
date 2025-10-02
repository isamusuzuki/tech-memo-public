# コンポーネントをrefなどのリアクティブ変数に格納する意味

作成日 2025/09/25、更新日 2025/10/02

## 1. AIによる概要

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

## 2. defineExposeと組み合わせる

解説記事 => [Vue の defineExpose でコンポーネント内部のスクロールを外部から制御する](https://zenn.dev/studio/articles/0798456c99e89c)

> Vue3 のコンポーネントでは、defineExpose を活用することで、親コンポーネントから子コンポーネント内の処理（例: スクロール位置の制御）を簡潔に、かつ、型安全に呼び出せます。

ChildComponent.vue

```html
<script setup lang="ts">
const scrollContainer = ref<HTMLDivElement | null>(null)

function scrollToTop(){
  if (scrollContainer.value) {
    scrollContainer.value.scrollTop = 0
  }
}

defineExpose({ scrollToTop })
</script>
```

ParentComponent.vue

```html
<script setup lang="ts">
import ChildComponent from '@/components/ChildComponent.vue'
type ChildInstance = InstanceType<typeof ChildComponent>
const childRef = ref<ChildInstance | null>(null)

function handlScrollToTop() {
  childRef.value?.scrollToTop()
}
</script>
```

### 公式ドキュメントを読む

[&lt;script setup&gt; > defineExpose()](https://ja.vuejs.org/api/sfc-script-setup#defineexpose)

- `<script setup>` を使用したコンポーネントは、デフォルトで閉じられています。
- `<script setup>` コンポーネントのプロパティを明示的に公開するには、defineExpose コンパイラーマクロを使用します。
