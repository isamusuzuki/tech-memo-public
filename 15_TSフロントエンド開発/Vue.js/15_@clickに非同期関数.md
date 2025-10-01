# `@click`に非同期関数？

作成日 2025/07/31

## 1. ギモン

コンポーネントの`@click`ディレクティブに非同期関数を紐づけても問題はないのか？

状態管理はどうなっているのか？

## 2. 公式サイトを読む

### 2a. [状態管理](https://ja.vuejs.org/guide/scaling-up/state-management.html)

全ての Vue コンポーネントインスタンスは、すでに自身のリアクティブな状態（state）を "管理" している

```html
<script setup>
import { ref } from 'vue'

// 状態
const count = ref(0)

// アクション
function increment() {
  count.value++
}
</script>

<!-- ビュー -->
<template>{{ count }}</template>
```

これは「アクション -> 状態 -> ビュー」の単一方向のデータフローをシンプルに表現している

### 2b. [ビルトインのディレクティブ](https://ja.vuejs.org/api/built-in-directives.html)

v-on ... 要素のイベントリスナーを追加します

```html
<!-- メソッドハンドラー -->
<button v-on:click="doThis"></button>

<!-- インラインステートメント -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 省略記法 -->
<button @click="doThis"></button>
```

### 2c. [イベントハンドリング](https://ja.vuejs.org/guide/essentials/event-handling.html)

v-on ディレクティブを使用することで、 DOMイベントの購読やイベント発火時にいくつかのJavaScriptを実行する

## 3. AIによる回答

Vue.jsで非同期関数をイベントハンドラとして使用する際、`async`関数を直接`v-on`ディレクティブに渡すことで、`await`を使用した非同期処理をイベントハンドラ内で実行できます。

```html
<template>
  <button @click="handleClick">Click me</button>
</template>

<script>
export default {
  methods: {
    async handleClick() {
      // 非同期処理
      const result = await this.fetchData();
      console.log(result);
    },
    async fetchData() {
      // 実際にはAPI呼び出しなどを行う
      return new Promise(resolve => {
        setTimeout(() => {
          resolve('Data fetched!');
        }, 1000);
      });
    }
  }
}
</script>
```

## 4. 結論

Piniaのような状態管理ライブラリが必要になるのは、複数のコンポーネントで状態を共有しようとしたときであり、単独のコンポーネントであれば、状態管理は考えなくてよい
