# Cloudflare Workersとは

作成日 2025/09/10、更新日 2025/09/16

## 1. 公式サイトを読む

[Cloudflare Workers | サーバーレスアプリケーションを構築 | Cloudflare](https://www.cloudflare.com/ja-jp/developer-platform/products/workers/)

> Cloudflareのグローバルネットワークの全世界330か所以上のデータセンターへのグローバルにデプロイ時にインフラの設定や保守は無用でサーバーレス機能やアプリケーションを構築。
>
>- Cloudflare WorkersはコンテナではなくV8分離で稼働
>- 起動時にインスタンスへのランタイム読み込みを待つ遅延が一切ない
>- 瞬時のスタートアップタイムが実現
>- 必要なのは呼び出し時に読み込まれるコードのみ
>- 実質的にリクエスト時のコールドスタートが存在しない

## 2. 新しいプロジェクトを開始する

[Get started - CLI · Cloudflare Workers docs](https://developers.cloudflare.com/workers/get-started/guide/)

```bash
npm create cloudflare@latest -- avocado
# Need to install the following packages:
# create-cloudflare@2.51.6
# Ok to proceed? (y) y

# Create an application with Cloudflare Step 1 of 3
#
# In which directory do you want to create your application?
# dir ./avocado
#
# What would you like to start with?
# category Hello World example
#
# Which template would you like to use?
# type SSR / full-stack app
#
# Which language do you want to use?
# lang TypeScript
#
# 中略
#
# Do you want to use git for version control?
# no git
# 
# Do you want to deploy your application?
# no deploy via `npm run deploy`
#
# Done

cd avocado
npm run dev #=> Open browser to http://localhost:8787/
```

## 3. サンプルアプリのファイル＆フォルダ構造

Hello World example, SSR/full-stack app,TypeScriptを選択した結果

```text
--avocado/
    |--public/
    |   `--index.html  ... スタティックファイル
    |--src/
    |   `--index.ts  ... Workerファイル
    |--worker-configuration.d.ts ... Workerファイルのための型定義
    `--wrangler.jsonc
```

wrangler.jsonc

```json
{
    "$schema": "node_modules/wrangler/config-schema.json",
    "name": "avocado",
    "main": "src/index.ts",
    "compatibility_date": "2025-09-13",
    "compatibility_flags": [
        "global_fetch_strictly_public"
    ],
    "assets": {
        "directory": "./public"
    },
    "observability": {
        "enabled": true
    }
}
```
