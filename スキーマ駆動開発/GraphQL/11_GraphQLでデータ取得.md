# GraphQL でデータ取得

作成日 2025/01/21、更新日 2025/03/18

参照サイト: [今更GraphQL：Query書き方基礎の基礎](https://zenn.dev/soma3134/articles/cb6b9de2564fb6)

## queryの書き方

```javascript
query ExampleQuery {
  allFilms {
    films {
      title
    }
  }
}
```

- `operation type` ... [query,mutation,subscription] queryの時は省略可
- `operation name` ... 省略可

```text
query 名前 {
    サーバーエンドポイント (引数キーバリュー)
    { 欲しい値 }
}
```

## 2. 戻り値

```json
{
  "data": {
    "allFilms": {
      "films": [
        {
          "title": "A New Hope"
        },
        {
          "title": "The Empire Strikes Back"
        }
    }
  }
}
```
