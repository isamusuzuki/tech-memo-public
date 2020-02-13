# Puppeteerを使いこなす

作成日 2020/02/10

## 01. ページサイズを変更する

Puppeteerのイニシャルのページサイズは、800x600

```javascript
const page = await browser.newPage();
page.setViewport({ width: 1920, height: 1080 });
```

## 02. ユーザーエージェントを変更する

```javascript
const page = await browser.newPage();
const chrome_win10 =
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36";
page.setUserAgent(chrome_win10);
```

## 03. ページ遷移する

```javascript
await page.goto('https://www.example.com/', 
    { waitUntil: 'networkidle2' }
);
```

## 04. しばし待つ

```javascript
// 3秒待つ
await page.waitFor(3000);
```

## 05. XPathを使いこなす

### 要素内のテキストを取得する

```javascript
const titles = await page.$x("//xpath");
const title = await page.evaluate(x => x.textContent, titles[0]);
```

`page.$x(expression)`の戻値は、`<Promise<Array<ElementHandle>>>`である \
たとえ1個しか見つからなくても配列なので、ゼロ番目を抜き出す必要がある

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
    await page.waitForNavigation(
        { waitUntil: 'networkidle2' }
    );
}
```

## 06. ドロップダウンリストの選択を変更する

これだけXPathで実現できていない。CSSセレクタを使う

```html
<label>Choose One:
    <select id="choose1">
        <option value="val1">Value 1</option>
        <option value="val2">Value 2</option>
        <option value="val3">Value 3</option>
    </select>
</label>
```

```javascript
await page.select('select#choose1', 'val2');
```

## 07. ポップアップ画面に移動する

```javascript
// ポップアップ画面を出す
const link1 = await page.$x('//xpath');
await link1[0].click();
await page.waitFor(1000);

// ポップアップ画面は、最後のページであるはず
const pages = await browser.pages();
const popup1 = pages[pages.length - 1];

// ポップアップ画面を閉じる
const close_button1 = await popup1.$x('//xpath');
close_button1[0].click();

// 元のページに戻る
await page.bringToFront();
```

## 08. 本当に画像が表示されているかを調べる

NaturalWidthを使う

![](https://imgur.com/51gXQYZ.png)

```javascript
const anpan = await page.$x('//img[@id="anpan"]');
const anpan_nw = await page.evaluate(x => x.naturalWidth, anpan[0]);
console.log(`anpan_nw => ${anpan_nw}`);

const curry = await page.$x('//img[@id="curry"]');
const curry_nw = await page.evaluate(x => x.naturalWidth, curry[0]);
console.log(`curry_nw => ${curry_nw}`);

const baikin = await page.$x('//img[@id="baikin"]');
const baikin_nw = await page.evaluate(x => x.naturalWidth, baikin[0]);
console.log(`baikin_nw => ${baikin_nw}`);
```

```text
anpan_nw => 240
curry_nw => 213
baikin_nw => 0
```
