# XPath を使いこなす

作成日 2020/02/10

## 01. XPathを使ってページから要素を抜き出して何かする

### 要素内のテキストを取得する

```javascript
const titles = await page.$x('//xpath');
const title = await page.evaluate((x) => x.textContent, titles[0]);
```

`page.$x(expression)`の戻値は、`<Promise<Array<ElementHandle>>>`である \
たとえ 1 個しか見つからなくても配列なので、ゼロ番目を抜き出す必要がある

### テキストボックスに文言を書き込む

```javascript
const mail = await page.$x('//input[@id="mail"]');
await mail[0].type(username_input, { delay: 30 });
```

### ボタンをクリックする

```javascript
const button = await page.$x('//input[@value="ログイン"]');
await button[0].click();
```

#### 確実にボタンがあるとは言えないときの対処法

```javascript
// 「次へのページ」リンクを探す
const nextLinks = await page.$x('//li[@class="a-last"]/a');

if (nextLinks.length > 0) {
  // リンクがあるなら遷移する
  await nextLinks[0].click();
  await page.waitForNavigation({ waitUntil: 'networkidle2' });
}
```

## 02. XPath の文法

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

## 03. XPath Helper とは

実際のページで、XPath を検証できる便利な Chrome 拡張機能

[https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl?hl=ja](https://chrome.google.com/webstore/detail/xpath-helper/hgimnogjllphhhkhlmebbmlgjoejdpjl?hl=ja)
