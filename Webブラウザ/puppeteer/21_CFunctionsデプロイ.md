# Cloud Functions にデプロイする

作成日 2020/02/10

## 01. コードの構造

```text
--functions/
    |--apps/
    |    `--scraper.js
    `--index.js
```

index.js

```javascript
const scraper = require("./apps/scraper");

exports.avocado = async (req, res) => {
  if (req.method === "POST") {
    if (req.body.name !== undefined) {
      const result = await scraper(req.body.name);
      res.json(result);
    } else {
      res.status(400).json({ message: "Bad Request" });
    }
  } else {
    res.status(405).json({ message: "Method Not Allowed" });
  }
```

apps/scraper.js

```javascript
const puppeteer = require('puppeteer');

module.exports = async (name) => {
  const url = `https://www.example.com/${name}/`;

  // ブラウザを準備する
  const browser = await puppeteer.launch({ args: ['--no-sandbox'] });
  const page = await browser.newPage();
  const chrome_win10 =
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36';
  page.setUserAgent(chrome_win10);
  page.setViewport({ width: 1920, height: 1080 });

  // 指定されたページを開く
  await page.goto(url, {
    waitUntil: 'domcontentloaded',
  });

  const result = {};

  // 商品名を取得する
  const titles = await page.$x('//xpath');
  const title = await page.evaluate((x) => x.textContent, titles[0]);
  result['title'] = title.trim();

  // ブラウザを閉じる
  await browser.close();

  return result;
};
```

## 02. デプロイ方法

デプロイ名と同名の関数がある index.js ファイルがあるフォルダ上で、\
gcloud コマンドを実行する。Puppeteer を実行するには、最低でも 1GB のメモリが必要

```bash
gcloud functions deploy avocado \
--region asia-northeast1 --memory 1024MB \
--runtime nodejs10 --trigger-http
```
