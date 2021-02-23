# CSS セレクタを勉強する

作成日 2020/02/10

MDN の解説ページ => [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

## セレクタ

| type                 | syntax                   | comment                       |
| -------------------- | ------------------------ | ----------------------------- |
| Type セレクタ        | `eltname`                | 要素名が合致したもの全員      |
| Class セレクタ       | `.classname`             | class 属性が合致したもの全員  |
| ID セレクタ          | `#idname`                | id 属性が合致。一意であるはず |
| ユニバーサルセレクタ | `*`                      | すべての要素が合致            |
| 属性セレクタ         | `[attr]`, `[attr=value]` | 属性で抽出                    |

## コンビネーション

| syntax  | comment                                                    |
| ------- | ---------------------------------------------------------- |
| `A B`   | A の中にある B をすべて抽出                                |
| `A > B` | A の直接の子供である B をすべて抽出                        |
| `A ~ B` | A の直接または間接的に連なる B をすべて抽出。A と B は親戚 |
| `A + B` | A の直後にくる B を抽出。一意のはず。A と B は同じ親の兄弟 |
