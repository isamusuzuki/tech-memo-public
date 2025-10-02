# graphql-requestを試す

作成日 2025/10/02

## ローカルのGraphQLサーバーにアクセスする

[CfWorkers統合試す](../b_GraphQLサーバー/14_CfWorkers統合試す.md)に登場するcacaoフォルダの隣にdaikonフォルダを作成する

`http://127.0.0.1:8787`が稼働している状態で、Node.jsスクリプトを動かして、データ取得を成功させるのがゴール

```bash
mkdir daikon
cd daikon
npm init -y
npm i graphql graphql-request
```

package.jsonを変更 => `{"type": "module"}`

index.js

```javascript
import { gql, GraphQLClient } from 'graphql-request';

const document = gql`
query GetMovie {
  movies {
    id
    rating
    releaseDate
    title
  }
}
`;

const endpoint = 'http://127.0.0.1:8787';
const client = new GraphQLClient(endpoint);

const result = await client.request(document);
console.log(result);
```

スクリプトを実行 => `node index.js`

成功

```text
{
  movies: [
    {
      id: '1',
      rating: 5,
      releaseDate: '1999-03-24',
      title: 'The Matrix'
    }
  ]
}
```
