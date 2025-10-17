# BetterAuthを試す

作成日 2025/10/17

## 1. React Router向けのサンプルコードとその解説記事を発見

[React Router + Cloudflare + Better Auth フルスタックボイラープレートの紹介](https://zenn.dev/atman/articles/360c1ea325d2cb)

リポジトリ => [atman-33/react-router-cloudflare-boilerplate](https://github.com/atman-33/react-router-cloudflare-boilerplate)

```bash
# リポジトリをクローン
git clone https://github.com/atman-33/react-router-cloudflare-boilerplate.git

cd react-router-cloudflare-boilerplate

# 依存関係をインストール
npm install

# 環境変数を設定
cp .env.example .dev.vars
cp .env.example .env

# データベースのマイグレーション
npm run auth:db:generate
npm run db:migrate

# デバッグ開始
npm run dev
# Open browser to http://localhost:5173

# D1データベースを作成
npx wrangler d1 create {db-name}
npm run db:migrate-production

# デプロイ
npm run deploy
```
