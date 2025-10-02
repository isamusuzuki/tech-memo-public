# graphqlパッケージの正体

作成日 2025/10/01

## 1. 公式サイトを読む

[graphql - npm](https://www.npmjs.com/package/graphql)

> GraphQL.js
>
> The JavaScript reference implementation for GraphQL, a query language for APIs created by Facebook.
>
> GraphQL.js provides two important capabilities: building a type schema and serving queries against that type schema.

GraphQL.jsは、重要な機能を2つ提供する。それは、typeスキーマを構築する機能と、typeスキーマに対してクエリを与えることである

### 自分の理解

GraphQL.jsは、あらゆるGraphQLライブラリの中心にあって、人間or機械によって生成されたGraphQLスキーマとGraphQLクエリードキュメントを解釈する仕事を担っている
