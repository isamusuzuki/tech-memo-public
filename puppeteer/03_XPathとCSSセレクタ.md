# XPathとCSSセレクタ

作成日 2020/02/10

## 01. XPathを勉強する

参考記事 => [https://qiita.com/rllllho/items/cb1187cec0fb17fc650a](https://qiita.com/rllllho/items/cb1187cec0fb17fc650a)

| syntax                                                   | comment                                |
| -------------------------------------------------------- | -------------------------------------- |
| `/html/body/h1`                                          | ツリー構造の上から順に要素を指定する   |
| `/html/body/div/span[@class='regular_price']`            | 属性を指定する                         |
| `//span[@class='regular_price']`                         | 途中までのパスを省略する               |
| `//img[contains(@class, 'image')]`                       | 属性に指定する文字列が含まれている     |
| `//div[contains(text(), '品番')]`                        | テキストに指定する文字列が含まれている |
| `//li[position()=3]` `//li[3]`                           | 要素の順番を指定（1 始まり）           |
| `//li[position()>1]`                                     | 要素を比較で指定                       |
| `//p/text()`                                             | 文字列を取得する                       |
| `//img[not(contains(@src, 'main'))]/@src`                | 否定してから属性を取得する             |
| `//td[contains(@src, '100') or contains(@src, '300')]`   | OR 条件を指定する                      |
| `//td[contains(@src, 'main') and contains(@src, '300')]` | AND 条件を指定する                     |
| `//td[contains(*, '素材')]/following-sibling::td[1]`     | 指定した要素よりも後の兄弟要素         |
| `//td[@class="inventory"][1]/preceding-sibling::td`      | 指定した要素よりも前の兄弟要素         |
| `//td[note(.=preceding::td)]`                            | 重複なく抽出                           |

### XPath Helperとは

実際のページで、XPathを検証できる便利な拡張機能

[https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl?hl=ja](https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl?hl=ja)

## 02. CSSセレクタを勉強する

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

