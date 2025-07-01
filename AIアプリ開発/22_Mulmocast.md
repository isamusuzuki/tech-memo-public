
# MulmocastをAIアプリ開発の手本とする

作成日 2025/07/01

## 1. Mulmocastとは

解説記事 => [プレゼン動画自動生成ツール Mulmocast を使う](https://zenn.dev/open_developers/articles/87928c78f98210)

> MulmoCastは、AIと人間が協力してアイデアを生み出し、共有するために設計された次世代のプレゼンテーションプラットフォームです。

### 1a. オリジナルテンプレートの作成手順

```bash
# レポジトリをクローン
gh repo clone receptron/mulmocast-cli

# 依存パッケージをインストール
pnpm install
pnpm add openai

# スクリプトを生成
 pnpm run scripting -u {元ネタのURL}  -t {テンプレートファイルの名前、/assets/templatesに入っています}

# 動画の生成
pnpm run movie {スクリプトファイルのパス}

# 環境変数を設定
OPENAI_API_KEY=
DEFAULT_OPENAI_IMAGE_MODEL=gpt-image-1
BROWSERLESS_API_TOKEN=
GOOGLE_PROJECT_ID=
NIJIVOICE_API_KEY=
```

mulmocastは、MulmoScriptというJSON形式の台本（というかシナリオ）のようなものを追加することでテンプレートを作成できます。

元ネタ（英語）を発見 => [mulmocast-cli/CONTRIBUTING.md](https://github.com/receptron/mulmocast-cli/blob/main/CONTRIBUTING.md)

## 2. ソースコードを読む

[receptron/mulmocast-cli: AI-powered podcast & video generator.](https://github.com/receptron/mulmocast-cli)
