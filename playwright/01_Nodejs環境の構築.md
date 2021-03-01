# Node.js 環境の構築

作成日 2021/02/28

## 01. Playwright をインストールする

参照した公式サイト => [Installation \| Playwright](https://playwright.dev/docs/installation)

Node.js LTS v14 がインストールされていることが前提

```bash
cd ~
mkdir playwright-dojo
cd ~/playwright-dojo
npm init -y

node -v
# => v14.16.0

npm -v
# => 7.6.0

# Chromium/Webkit/Firefox の3つのブラウザをインストールするので時間がかかる
# npm i -D playwright

# Chromiumだけをインストールする派生バージョンを使う
npm i -D playwright-chromium
```

### インストールされたブラウザのありか

- Windows の場合 ... `~\AppData\Local\ms-playwright`
- Linux の場合 ... `~/.cache/ms-playwright`

インストール後のファイル構造を確認する

```text
--.cache/
    `--ms-playwright/
        |--chromium-854489/
        |   |--INSTALLATION_COMPELTE
        |   `--chrome-linux/
        |       `--chrome
        `--ffmpeg-1005/
```

### インストールされた Playwright のバージョンを確認する

```bash
npx playwright --version
# => Version 1.9.1
```

### Node.js コードの動作確認

firstscript.js

```javascript
const { chromium } = require('playwright-chromium');

(async () => {
    const browser = await chromium.launch({
        headless: false,
        slowMo: 50,
    });
    const page = await browser.newPage();
    await page.goto('http://whatsmyuseragent.org/');
    await page.screenshot({ path: 'temp/firstscript.js.png' });
    await browser.close();
})();
```
