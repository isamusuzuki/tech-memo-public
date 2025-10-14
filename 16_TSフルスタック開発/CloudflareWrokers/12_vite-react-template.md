# vite-react-template

作成日 2025/09/16、更新日 2025/10/14

## 1. リポジトリを見る

[cloudflare/templates: Templates for Cloudflare Workers](https://github.com/cloudflare/templates)

> This repository contains a collection of starter templates for building full-stack applications on Workers.

このリポジトリにはワーカーで動作するフルスタックアプリケーションを構築するためのスターターテンプレート集が含まれている

Cloudflare DocsのFramework guidesにある[Honoのページ](https://developers.cloudflare.com/workers/framework-guides/web-apps/more-web-frameworks/hono/)を開くと、次のテンプレートが紹介されている

[templates/vite-react-template](https://github.com/cloudflare/templates/tree/main/vite-react-template)

> React + Vite + Hono + Cloudflare Workers
>
> This template provides a minimal setup for building a React application with TypeScript and Vite, designed to run on Cloudflare Workers. It features hot module replacement, ESLint integration, and the flexibility of Workers deployments.

## 2. vite-react-templateを試す

### プロジェクトの開始

```bash
node -v
# v22.14.0
npm -v
# 11.4.2

npm create cloudflare@latest -- --template=cloudflare/templates/vite-react-template
# dir ./avocado
# no git
# no deploy
```

### 微修正

- `worker-configuration.d.ts`の2行目に`// @ts-nocheck`を追加する

開発コンテナを使っている場合

- `.vscode/setting.json`の内容を、開発コンテナの設定に書き写す
- `wrangler.json`に、`server: { host: true }`を追加する

### デバッグを開始

```bash
cd avocado
npm run dev # open browser to http://localhost:5173/
```

- 開発サーバーは、Wranglerではなく、Viteを使う
- [Cloudflare Vite plugin](https://developers.cloudflare.com/workers/vite-plugin/)が、ViteとWorkers runtimeを統合する
- 開発サーバーは、Workers runtimeも稼働させる
- Viteの環境変数を、ViteとWorkers runtimeで共用する

### ファイル＆フォルダ構成を確認する

```text
--avocado/
    |--dist/            ... ビルドの出力先
    |   |--avocado/
    |   |   |--index.js ... 本番Worker
    |   `--client/      ... 静的ファイルの置き場
    |       |--assets/
    |       `--index.html
    |--public/          ... dist/client/へ直行
    |--src/
    |   |--react-app/.  ... Reactアプリ
    |   |   |--App.tsx
    |   |   `--main.tsx
    |   `--worker/
    |       `--index.ts ... Honoアプリ
    `--index.html
    `--wrangler.json ... Cloudflareの設定
```

wrangler.json

```json
{
    "assets": {
        "directory": "./dist/client/",
        "not_found_handling": "single-page-application"
    }
}
```

### Cloudflareにデプロイしてみる

- .envファイルに、`CLOUDFLARE_API_TOKEN=xxx`を書き込む
- wrangler.jsonファイルに、`"account_id": "xxx"`を書き込む

デプロイ作業は、distフォルダにあるwrangler.jsonを利用して行われる。あらかじめビルドしておく必要があった

```bash
npm run build
npm run deploy
```

成功！
