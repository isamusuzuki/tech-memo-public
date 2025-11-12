# TanStack Query

作成日 2025/09/03、更新日 2025/11/12

React,Vue.jsの状態管理ライブラリ

## 1. 解説記事を読む

[TanStack Query　〜プロダクトで採用するための勘所〜](https://zenn.dev/taisei_13046/books/133e9995b6aadf)

> TanStack Queryはただのデータフェッチライブラリではありません。サーバから取得したデータを効率的に扱うための様々な機能を持った非同期の状態管理ライブラリです。TanStack Queryを使用することで従来の状態管理ライブラリと比べ大幅にコードを削減できます。

## 2. 公式サイト（英語）を読む

[Overview | TanStack Query Vue Docs](https://tanstack.com/query/latest/docs/framework/vue/overview)

インストール => `npm install --save @tanstack/vue-query`

## 3. サンプルコードを試す

[Vue TanStack Query Basic Example | TanStack Query Docs](https://tanstack.com/query/latest/docs/framework/vue/examples/basic)

Posts.vue

```html
<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query'

const fetcher = async (): Promise<Post[]> =>
  await fetch('https://jsonplaceholder.typicode.com/posts').then((response) =>
    response.json(),
  )

const { isPending, isError, data, error } = useQuery({
  queryKey: ['posts'],
  queryFn: fetcher,
})
</script>

<template>
  <h1>Posts</h1>
  <div v-if="isPending">Loading...</div>
  <div v-else-if="isError">An error has occurred: {{ error }}</div>
  <div v-else-if="data">
    <ul>
      <li v-for="item in data" :key="item.id">
        {{ item.title }}
      </li>
    </ul>
  </div>
</template>
```

## 4. queryKeyとは何か？

[Queries > Query Basics](https://tanstack.com/query/latest/docs/framework/vue/guides/queries#query-basics)

コンポーネントもしくはカスタムフックでクエリーを登録したい場合は、`useQuery`フックを呼ぶ。その時必要なものは、(1) そのクエリーのユニークなキー、(2) Promiseを返す関数の２つである

```javascript
import { useQuery } from '@tanstack/vue-query'

const result = useQuery({ queryKey: ['todos'], queryFn: fetchTodoList })
```

resultオブジェクトには、状態と、その状態によって取得可能なプロパティが含まれる

- 状態: `isPending`, `isError`, `isSuccess`
- `error`: `isError`の時だけ取得可能
- `data`: `isSuccess`の時だけ取得可能

```javascript
const { isPending, isError, data, error } = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
```
