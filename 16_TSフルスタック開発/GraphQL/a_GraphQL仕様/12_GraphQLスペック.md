# GraphQLスペック

作成日 2025/06/11

## 1. [Working Draft](https://spec.graphql.org/draft/)

ここに書かれていない定義はない

- 2.3 Operations
- 2.8 Fragments
- 2.9 Input
- 3.3 Schemas
- 3.4 Types
- 3.5 Scalars
- 3.6 Objects
- 3.10 Input Objects

## 2. [Introduction to GraphQL](https://graphql.org/learn/)

- [Schemas and Types](https://graphql.org/learn/schema/)
- [Queries](https://graphql.org/learn/queries/)

## 3. Fragmentとスプレッドオペレーター

参照記事 => [GraphQLのFragmentについての話](https://zenn.dev/sjbworks/articles/0b34ce8aca6b72)

参照記事の元ネタ => [Fragments](https://www.apollographql.com/docs/react/data/fragments)

フラグメントは、フィールドのセット（集まり）で、複数のクエリーとミューテーションで再利用が可能

以下はNamePartsフラグメントの宣言で、スプレッドオペレーター（`...`）を頭につけることで、Personオブジェクトを参照する箇所で、利用できる

```javascript
fragment NameParts on Person {
    firstName
    lastName
}

query GetPerson {
    people(id: "7") {
        ...NameParts
        avatar(size: LARGE)
    }
}
```
