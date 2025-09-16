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

## 2. プロジェクトを開始する

[Get started - CLI · Cloudflare Workers docs](https://developers.cloudflare.com/workers/get-started/guide/)

```bash
npm create cloudflare@latest -- my-first-worker
cd my-first-worker
npx wrangler dev # Open browser to http://localhost:8787
```
