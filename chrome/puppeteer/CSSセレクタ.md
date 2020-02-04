---
tags: puppeteer
---

# CSSセレクタ

作成日: 2019/11/28

## 01. CSSセレクタを勉強する

MDN の解説ページ => [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

### セレクタ

| type                | syntax                   | comment                 |
| ------------------- | ------------------------ | ----------------------- |
| Typeセレクタ         | `eltname`                | 要素名が合致したもの全員     |
| Classセレクタ        | `.classname`             | class属性が合致したもの全員 |
| IDセレクタ           | `#idname`                | id属性が合致。一意であるはず |
| ユニバーサルセレクタ   | `*`                      | すべての要素が合致          |
| 属性セレクタ          | `[attr]`, `[attr=value]` | 属性で抽出                |

### コンビネーション

| syntax  | comment                                     |
| ------- | ------------------------------------------- |
| `A B`   | Aの中にあるBをすべて抽出                        |
| `A > B` | Aの直接の子供であるBをすべて抽出                  |
| `A ~ B` | Aの直接または間接的に連なるBをすべて抽出。AとBは親戚 |
| `A + B` | Aの直後にくるBを抽出。一意のはず。AとBは同じ親の兄弟 |

