# TanStack Queryの公式ガイドを読む

作成日 2025/11/12

## 1. [Queries](https://tanstack.com/query/latest/docs/framework/vue/guides/queries)

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

## 2. [Query Keys](https://tanstack.com/query/latest/docs/framework/vue/guides/query-keys)

TanStack Queryは、その内部では、クエリーキーを元にクエリーのキャッシングを管理している。クエリーキーは、配列でなければならず、最も単純な形として、ひとつの文字列の配列でも構わないし、複数の文字列の配列であったり、ネストしたオブジェクトの配列であったりといった複雑な形のものでも構わない。クエリーキーは`JSON.stringify`を使ってシリアライズ可能で、かつ、ユニークであればいい
