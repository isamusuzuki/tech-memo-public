# Variables

作成日 2025/10/02

## 公式ドキュメント（英語）を読む

[Queries > Variables](https://graphql.org/learn/queries/#variables)

変数を有効にしたいときは、次の3つのことを行う

1. クエリー内の固定値を`$variableName`に置き換える
2. `$variableName`はクエリー内で使える変数の1つとして宣言する（引数として？）
3. クエリーとは別に用意したJSONに、`variableName: value`変数辞書を書き込む

Operation

```javascript
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

Variables

```json
{
  "episode": "JEDI"
}
```
