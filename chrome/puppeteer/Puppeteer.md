---
tags: puppeteer
---

# Puppeteer

作成日 2019/11/25

## 01. Puppeteerとは

Chrome/Chromiumをヘッドレスで操作するNode.jsライブラリ

ドキュメント => [https://pptr.dev/](https://pptr.dev/)

リポジトリ => [https://github.com/puppeteer/puppeteer](https://github.com/puppeteer/puppeteer)

## 02. Windows 10に、Puppeteerをセットアップする

### Node.jsをインストールする

[Chocolatey](https://chocolatey.org/)を使う => `choco install nodejs-lts`

Gith Bash を開いて、確認する

```bash=
node -v
#=> v12.13.0

npm -v
#=> 6.12.0

npm list -g
# => C:\Users\user\AppData\Roaming\npm
# => `-- (empty)
```

### Puppeteer用のプロジェクトを作成する

Gith Bash を開く

```bash=
mkdir puppeteer-sandbox
cd puppeteer-sandbox

# pacakge.jsonを作成する
npm init -y

# puppeteerをインストールする
npm install puppeteer --save
# => Chormiumも一緒にインストールされる
```

### Visual Studio Codeを設定する

JavaScriptコードを自動で修正してくれる、Prettierという拡張機能をインストールする

[Prettier \- Code formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

どのように修正して欲しいかは、カスタマイズ可能

.prettierrc

```json=
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true
}
```

#### require 文にサジェストが表示される場合は、次の設定を追加する

```javascript=
const puppeteer = require("puppeteer");
```

.vscode/settings.json

```json=
{
  "javascript.suggestionActions.enabled": false
}
```

## 03. Puppeteerのもっとも簡単なコード

### スクリーンショットをPNG画像として保存する

```javascript=
const puppeteer = require("puppeteer");
const moment = require("moment");
const nowStr = moment().format("YYYYMMDD_HHmmss");

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto("https://www.example.com/");
  await page.screenshot({ path: `../temp/${nowStr}.png` });

  await browser.close();
  await console.log("done");
})().catch(e => console.error(e));
```

無名関数を書いた直後に実行させる場合は、行終わりのカンマが必須

### スクリーンショットをPDFファイルとして保存する

```javascript=
const puppeteer = require("puppeteer");

puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  await page.goto("https://www.example.com/");
  const title = await page.title();
  await page.pdf({ path: `../temp/${title}.pdf`, format: "A4" });

  await browser.close();
  await console.log("done");
})().catch(e => console.error(e));
```

無名関数の即時実行の代わりに、Promiseチェーンを使うこともできる
