# Page クラス

作成日 2020/02/10、更新日 2021/03/19

公式サイト => [Page](https://pptr.dev/#?product=Puppeteer&show=api-class-page)

## 01. ページサイズを変更する

Puppeteer のイニシャルのページサイズは、800x600

```javascript
const page = await browser.newPage();
page.setViewport({ width: 1920, height: 1080 });
```

## 02. ユーザーエージェントを変更する

```javascript
const page = await browser.newPage();
const chrome_win10 =
  'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36';
page.setUserAgent(chrome_win10);
```

## 03. ページ遷移する

```javascript
await page.goto(
  'https://www.example.com/', 
  { waitUntil: 'networkidle2' }
);
```

## 04. ポップアップ画面に移動する

```javascript
// ポップアップ画面を出す
const link1 = await page.$x('//xpath');
await link1[0].click();

// ポップアップ画面は、最後のページであるはず
const pages = await browser.pages();
const popup1 = pages[pages.length - 1];

// ポップアップ画面を閉じる
const close_button1 = await popup1.$x('//xpath');
close_button1[0].click();

// 元のページに戻る
await page.bringToFront();
```
