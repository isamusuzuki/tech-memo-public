# JSONデータ解析

作成日 2025/07/20

## 1. jq

公式サイト（英語） => [jq](https://jqlang.org/)

> jq is like sed for JSON data - you can use it to slice and filter and map and transform structured data with the same ease that sed, awk, grep and friends let you play with text.

解説記事 => [jqコマンドでJSONを操作・分析する](https://zenn.dev/oreo2990/articles/49a20a09517d90)

```bash
# `.`で全てのJSONデータを取得する
cat people.json | jq .

# `.特定の要素名`で特定の要素を種痘する
cat people.json | jq '.count'

# `.配列の要素名[]`で配列の要素を取得する
cat people.json | jq '.results[]'

# `.配列名[].特定の要素名`で配列内の特定の要素を取得する
cat people.json | jq '.results[].name'

# `select(boolean_expression)`で絞り込みが可能
cat people.json | jq '.results[] | select( .gender == "male") | .name'
```

リアルタイムでjqコマンドの検証ができるサイト => [jq playground](https://play.jqlang.org/s/KsJNDSeALRk)

## 2. JSONPath

公式サイト（英語） => [JSONPath - XPath for JSON](https://goessner.net/articles/JsonPath/)

- XPath expression => `/store/book[1]/title`
- JSONPath expression => `$.store.book[0].title`

解説記事 => [JSONPath 使い方まとめ](https://qiita.com/takkii1010/items/0ce1c834d3a73496ccef)
