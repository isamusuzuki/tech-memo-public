# Headless Recorder

作成日 2021/03/19

## Headless Recorder とは

Checkly がリリースしている Chrome 機能拡張。ブラウザの操作を記録して、Puppeteer と Playwright のコードを自動生成する

[Headless Recorder \- Chrome ウェブストア](https://chrome.google.com/webstore/detail/headless-recorder/djeegiggegleadkkbgopoonhjimgehda)

## Playwright と Puppeteer の違い

### 1. コードの外枠

Playwright

```javascript
const { chromium } = require('playwright');
(async () => {
  const browser = await chromium.launch();
  const context = await browser.newContext();
  const page = await context.newPage();

  // 自動生成されたコードが入る

  await browser.close();
})();
```

Puppeteer

```javascript
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  // 自動生成されたコードが入る

  await browser.close();
})();
```

### 2. ブラウザのウィンドウサイズ指定

```javascript
// Playwright
await page.setViewportSize({ width: 1280, height: 800 });

// Puppeteer
await page.setViewport({ width: 1280, height: 800 });
```

### 3. リストの選択

```javascript
// Playwright
await page.selectOption('#my_contents_body #csv_type', '4');

// Puppeteer
await page.select('#my_contents_body #csv_type', '4');
```

## 現時点での結論

これを使って、やりたい作業を一通り記録すれば、何も考えずに「CSS セレクター」が決まるのがありがたい

やはり、開発は Node.js で行うべきだろう。Playwright ならば、そこから Python に移植できるし、もし Cloud Functions にデプロイするならば、Puppeteer に変更すればいい
