# Cloudflare Workersとの統合を試す

作成日 2025/10/02

## テンプレートを動かしてみる

[cloudflare/workers-graphql-server: 🔥Lightning-fast, globally distributed Apollo GraphQL server, deployed at the edge using Cloudflare Workers](https://github.com/cloudflare/workers-graphql-server)

### セットアップ

GitHubリポジトリのページにある緑色のCodeボタンをクリック ＞ Download ZIPをクリック

ファイルを展開して、workers-graphql-server-templateフォルダをcacaoに名前変更して、フォルダを移動

```bash
cd cacao
npm ci
```

### GraphQLCodegenを走らせる

```bash
npm run codegen
# => src/generated/graphql.ts 8.28KB
# => graphql.schema.json 45KB

npm run dev #=> Open brwoser to http://127.0.0.1:8787
```

Apollo Sandboxが登場

Operation 枠の左にDocumentation枠があり、そこを使うとラクにクエリーを作成できる

```javascript
query GetMovie {
  movies {
    id
    rating
    releaseDate
    title
  }
}

mutation add {
  add (
    numberOne: 11,
    numberTwo: 23
    )
}

query GetPokemon {
  pokemon(id: 3) {
    id
    name
    weight
    height
    sprites {
      back_default
      front_default
    }
  }
}
```

## 理解したこと

- `npm i @apollo/server`を実行すると、リリースされたばかりのv5がインストールされる。Cloudflare Workers統合のパッケージは、まだこのバージョンに対応していない
- Apollo Server v4の最終バージョンは`4.12.2`である。v4系は2026年1月に引退する
