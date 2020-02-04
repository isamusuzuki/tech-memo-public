# GraphQL を学ぶ

作成日 2019/11/26

## 01. GraphQL とは

A query language for your API

公式トップ => [http://graphql.org/](http://graphql.org/)

イントロダクション => [https://graphql.org/learn/](https://graphql.org/learn/)

### Contentful が GraphQL を採用

[Gatsby](https://www.gatsbyjs.org/)という静的サイトジェネレーターで採用された、\
REST の次に来る問い合わせ言語\
[Contentful](https://www.contentful.com/)というヘッドレス CMS でも運用開始

[Beyond REST: Running GraphQL queries in Contentful](https://www.contentful.com/blog/2017/05/22/running-graphql-queries-in-contentful/)

## 02. GraphQL の紹介記事を読む

[GraphQL: Everything You Need to Know – Weblab Technology – Medium](https://medium.com/@weblab_tech/graphql-everything-you-need-to-know-58756ff253d8)

[GraphQL 入門 \- 使いたくなる GraphQL \- Qiita](https://qiita.com/bananaumai/items/3eb77a67102f53e8a1ad)

> -   GraphQL は、`Schema`（API の定義）を持つ
> -   `type`キーワードは、オブジェクト型を定義している
> -   `!`は、Not Null の指定キーワード
> -   `ID`フィールドは、ID 型という重複のない一意性が保証された特別な文字列
> -   `Query`で指定されたフィールドは、API のクエリー用インターフェイスになる
> -   `Mutation`で指定されたフィールドは、データ操作用の API を定義する

```text
schema {
    type Shop {
        id: ID!
        name: String!
    }
    type Query {
        shops: [Shop]!
        shop(name: String): Shop
    }
    type Mutation {
        addShop(shop: shopInput): Shop
        updateShop(id: ID!, shop): ShopInput
    }
    input ShopInput {
        name: Name
    }
}

query {
    shops {
        id,
        name
    }
}

query {
    shop(name: "果物屋") {
        id
    }
}

mutation {
    addShop({name: "八百屋"}) {
        id
    }
}
```

## 03. Graphiql

オンライン IDE

[graphql/graphiql: An in\-browser IDE for exploring GraphQL\.](https://github.com/graphql/graphiql)

## 04. GraphQL 対応のテストサーバー

[Public GraphQL APIs \| graphql\-apis](http://apis.guru/graphql-apis/)
