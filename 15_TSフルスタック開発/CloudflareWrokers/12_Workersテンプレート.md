# Workersテンプレート

作成日 2025/09/16

## 1. ソースコードを読む

[cloudflare/templates: Templates for Cloudflare Workers](https://github.com/cloudflare/templates)

> This repository contains a collection of starter templates for building full-stack applications on Workers.

## 2. vite-react-template

Cloudflare DocsのFramework guidesにある[Honoのページ](https://developers.cloudflare.com/workers/framework-guides/web-apps/more-web-frameworks/hono/)を開くと、このテンプレートが紹介されている

[templates/vite-react-template](https://github.com/cloudflare/templates/tree/main/vite-react-template)

> React + Vite + Hono + Cloudflare Workers
>
> This template provides a minimal setup for building a React application with TypeScript and Vite, designed to run on Cloudflare Workers. It features hot module replacement, ESLint integration, and the flexibility of Workers deployments.

```bash
npm create cloudflare@latest -- --template=cloudflare/templates/vite-react-template
npm install
npm run dev
# => http://localhost:5173 を開く
```

- 開発サーバーは、Wranglerではなく、Viteを使う
- [Cloudflare Vite plugin](https://developers.cloudflare.com/workers/vite-plugin/)が、ViteとWorkers runtimeを統合する
- 開発サーバーは、Workers runtimeも稼働させる
- Viteの環境変数を、ViteとWorkers runtimeで共用する
