# Puppeteer を使う

作成日 2020/02/10

## 01. Puppeteer とは

Chrome/Chromium をヘッドレスで操作する Node.js ライブラリ

ドキュメント => [https://pptr.dev/](https://pptr.dev/)

リポジトリ => [https://github.com/puppeteer/puppeteer](https://github.com/puppeteer/puppeteer)

## 02. Puppeteer をセットアップする

### Node.js をインストールする

[Chocolatey](https://chocolatey.org/)を使う => `choco install nodejs-lts　-y`

```bash
node -v
# => v12.15.0

npm -v
# => 6.13.4

npm list -g
# => C:\Users\user\AppData\Roaming\npm
# => `-- (empty)
```

### Puppeteer 用のプロジェクトを作成する

```bash
mkdir puppeteer-sandbox
cd puppeteer-sandbox

# pacakge.jsonを作成する
npm init -y

# puppeteerをインストールする
npm install puppeteer --save
# => Chormium も一緒にインストールされる
```

### Visual Studio Code を設定する

JavaScript コードを自動で修正してくれる、Prettier という拡張機能をインストールする

[Prettier \- Code formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

どのように修正して欲しいかは、カスタマイズ可能\
プロジェクトのルートディレクトリに`.prettierrc`というファイルを置く

```json
{
    "trailingComma": "es5",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true
}
```

#### require 文にサジェストが表示される場合は、次の設定を追加する

```javascript
const puppeteer = require('puppeteer');
```

.vscode/settings.json

```json
{
    "javascript.suggestionActions.enabled": false
}
```

## 03. Puppeteer のもっとも簡単なコード

### スクリーンショットを PNG 画像として保存する

```javascript
const puppeteer = require('puppeteer');
const moment = require('moment');
const nowStr = moment().format('YYYYMMDD_HHmmss');

(async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.goto('https://www.example.com/');
    await page.screenshot({ path: `../temp/${nowStr}.png` });

    await browser.close();
    await console.log('done');
})().catch(e => console.error(e));
```

無名関数を書いた直後に実行させる場合は、行終わりのカンマが必須

### スクリーンショットを PDF ファイルとして保存する

```javascript
const puppeteer = require('puppeteer');

puppeteer
    .launch()
    .then(async browser => {
        const page = await browser.newPage();
        await page.goto('https://www.example.com/');
        const title = await page.title();
        await page.pdf({ path: `../temp/${title}.pdf`, format: 'A4' });

        await browser.close();
        await console.log('done');
    })()
    .catch(e => console.error(e));
```

無名関数の即時実行の代わりに、Promise チェーンを使うこともできる
