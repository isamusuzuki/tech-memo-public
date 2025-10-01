# Cloudflare Workersとは

作成日 2025/09/10、更新日 2025/09/27

## 1. 公式サイトを読む

[Cloudflare Workers | サーバーレスアプリケーションを構築 | Cloudflare](https://www.cloudflare.com/ja-jp/developer-platform/products/workers/)

> Cloudflareのグローバルネットワークの全世界330か所以上のデータセンターへのグローバルにデプロイ時に\
> インフラの設定や保守は無用でサーバーレス機能やアプリケーションを構築。
>
>- Cloudflare WorkersはコンテナではなくV8分離で稼働
>- 起動時にインスタンスへのランタイム読み込みを待つ遅延が一切ない
>- 瞬時のスタートアップタイムが実現
>- 必要なのは呼び出し時に読み込まれるコードのみ
>- 実質的にリクエスト時のコールドスタートが存在しない

## 2. 新しいプロジェクトを開始する

[Get started - CLI · Cloudflare Workers docs](https://developers.cloudflare.com/workers/get-started/guide/)

```bash
npm create cloudflare@latest
# dir ./avocado
# category Hello World example
# type SSR / full-stack app
# lang TypeScript
# no git
# no deploy via `npm run deploy`

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

## 4. つまづいた所

cloudflare-workersの管理ツールであるwranglerは、開発サーバーも兼ねているが、\
開発コンテナの中で開発サーバーを利用するためには、Viteの時と同じように、\
外部PCからの開発サーバーへのアクセスを許可する必要がある

```json
{
    "dev": {
        "ip": "0.0.0.0"
    }
}
```
