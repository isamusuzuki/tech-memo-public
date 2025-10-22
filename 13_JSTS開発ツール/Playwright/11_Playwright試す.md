# Playwrightを試す

作成日 2025/10/07

## 1. Playwrightのインストール

```bash
cd browser-automation
bun create playwright
# Getting started with writing end-to-end tests with Playwright:
# Initializing project in '.'
# √ Do you want to use TypeScript or JavaScript? · TypeScript
# √ Where to put your end-to-end tests? · tests
# √ Add a GitHub Actions workflow? (y/N) · false
# √ Install Playwright browsers (can be done manually via 'npx playwright install')? (Y/n) · true
```

ブラウザのインストールに`true`と答えたので、ブラウザが4種類インストールされた

- `C:\Users\{{username}}\AppData\Local\ms-playwright\chromium_headless_shell-1187`
- `C:\Users\{{username}}\AppData\Local\ms-playwright\chromium-1187`
- `C:\Users\{{username}}\AppData\Local\ms-playwright\firefox-1490`
- `C:\Users\{{username}}\AppData\Local\ms-playwright\webkit-2203`

## 2. Bunでnpmコマンドを実行する

```bash
# testsフォルダにあるテストをすべて実行する
bun x playwright test

# testsフォルダにあるテストをファイル名を指定して実行する
bun x playwright test example

# ブラウザを指定して実行する
bun x playwright test example --project=chromium

# ブラウザを表示して実行する
bun x playwright test example --debug

# UIモードでテストを実行する
bun x playwright test example --ui

# 直前のテストのレポートを見る http://localhost:9323を開く
bun x playwright show-report

# コードジェネレーターを使う
bun x playwright codegen
```

## 3. 環境変数を取り込む

dotenvをインストールする

```bash
bun add dotenv
```

playwright.config.tsにて、環境変数を取り込むことが可能

```javascript
// コメントアウトを外すだけ
import dotenv from 'dotenv';
import path from 'path';
dotenv.config({ path: path.resolve(__dirname, '.env') });
```

## 4. playwrighjt.config.tsをいじる

ブラウザ表示と言語ヘッダーを日本語にする

```javascript
export default defineConfig({
    // 末尾に以下を追加する
    use: {
        locale: 'ja-JP',
        extraHTTPHeaders: {
            'Accept-Language': 'ja-JP,ja;q=0.9',
        }
    }
})
```

使うブラウザをChromiumに限定する

```javascript
export default defineConfig({
    projects: [
        {
        name: 'chromium',
        use: { ...devices['Desktop Chrome'] },
        },
        // 他をコメントアウトする
    ]
});
```

## 4. テストコードの例

```javascript
import { test, expect } from '@playwright/test';

test('asana', async ({ page }) => {
  const email = process.env.EMAIL;
  const passwd = process.env.PASSWD;
  if (typeof email === 'undefined' || typeof passwd === 'undefined') {
    throw new Error('Missing environment variables');
  }
  await page.goto('https://example.com/ja');
  await page.getByRole('link').filter({ hasText: 'ログイン' }).click();
  await page.getByRole('textbox', { name: 'メールアドレス' }).fill(email);
  await page.getByRole('button', { name: '続行' }).click();
  await page.getByRole('textbox', { name: 'パスワード' }).fill(passwd);
  await page.getByRole('button', { name: 'ログイン' }).click();
  await page.goto('https://app.example.com/1/11999/home');
  await page.getByRole('link', { name: 'プロジェクト' }).click();
  await page.getByRole('button', { name: 'アクション', exact: true }).click();
  await page.getByText('同期 / エクスポート').hover();
  await page.getByText('プロジェクトのタスク (CSV)').click();
  const page1Promise = page.waitForEvent('popup');
  await page.getByRole('button', { name: 'CSV をエクスポート' }).click();
  const page1 = await page1Promise;
  await expect(page1.locator('pre')).toContainText('Your CSV is being generated. You will get an email when it is finished.');
});
```
