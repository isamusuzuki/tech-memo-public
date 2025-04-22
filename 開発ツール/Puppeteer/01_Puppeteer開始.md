# Puppeteer を開始する

作成日 2020/02/10、更新日 2022/01/12

## 01. Puppeteer とは

DevTools Protocol 経由で Chrome/Chromium を操作する Node ライブラリ

公式サイト => [https://pptr.dev/](https://pptr.dev/)

## 02. Puppeteer をセットアップする

### Node.js はすでにインストールされているものとする

```bash
node -v
# => v16.13.2

npm -v
# => 8.1.2
```

### 新規プロジェクトを作成する

```bash
mkdir avocado
cd avocado

# pacakge.jsonを作成する
npm init -y

# puppeteerをインストールする
npm i -S puppeteer
```

=> Chormium も一緒にインストールするので時間がかかる

### Chromium のありか

```text
--avocado/
  `--node_modules/
      `--puppeteer/
          `--.local-chromium/
                `--linux-856583/
                    `--chrome-linux/
                        `--chrome
```

## 03. Puppeteer の簡単なコードを書く

### スクリーンショットを PNG 画像として保存する

first.js

```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('http://whatsmyuseragent.org/');
  await page.screenshot({ path: `temp/test.png` });
  await browser.close();
  console.log('done');
})().catch((e) => console.error(e));
```

即時関数を使う場合は、行終わりのカンマが必須となる。カンマがないと、1 行目の `require()` メソッドと 3 行目冒頭のカッコが繋がってしまって、`TypeError: require(...) is not a function` というエラーを出す

```bash
cd ~/avocado

vi first.js

node first.js
```

### スクリーンショットを PDF ファイルとして保存する

second.js

```javascript
const puppeteer = require('puppeteer');

puppeteer
  .launch()
  .then(async (browser) => {
    const page = await browser.newPage();
    await page.goto('https://www.example.com/');
    const title = await page.title();
    await page.pdf({ path: `temp/${title}.pdf`, format: 'A4' });
    await browser.close();
    console.log('done');
  })
  .catch((e) => console.error(e));
```

即時関数の代わりに、Promise チェーンを使うこともできる
