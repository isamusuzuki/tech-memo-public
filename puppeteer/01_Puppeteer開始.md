# Puppeteer を開始する

作成日 2020/02/10、更新日 2021/03/19

## 01. Puppeteer とは

DevTools Protocol 経由で Chrome/Chromium を操作する Node ライブラリ

公式サイト => [https://pptr.dev/](https://pptr.dev/)

## 02. Puppeteer をセットアップする

### Node.js をインストールする

公式サイト => [Node\.js](https://nodejs.org/en/)

Windows の場合は、公式サイトから LTS 版のインストーラーをダウンロードする

```bash
node -v
# => v14.16.0

npm -v
# => 7.6.1
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

=> Chormium も一緒にインストールされるので時間がかかる

### Chromiumのありか

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

```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('http://whatsmyuseragent.org/');
  await page.screenshot({ path: `../temp/test.png` });
  await browser.close();
  console.log('done');
})().catch((e) => console.error(e));
```

無名関数を書いた直後に実行させる場合は、行終わりのカンマが必須

### スクリーンショットを PDF ファイルとして保存する

```javascript
const puppeteer = require('puppeteer');

puppeteer
  .launch()
  .then(async (browser) => {
    const page = await browser.newPage();
    await page.goto('https://www.example.com/');
    const title = await page.title();
    await page.pdf({ path: `../temp/${title}.pdf`, format: 'A4' });
    await browser.close();
    console.log('done');
  })
  .catch((e) => console.error(e));
```

無名関数の即時実行の代わりに、Promise チェーンを使うこともできる
