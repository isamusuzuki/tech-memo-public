# Spec Kit

作成日 2025/09/08

## 1. 参照記事を読む

[仕様駆動開発を支える Spec Kit を試してみた](https://azukiazusa.dev/blog/spec-driven-development-with-spec-kit/)

> GitHub が提供する Spec Kit は、仕様駆動開発を支援するためのツールキットであり、AI との対話を通じて正確な受け入れ基準の定義とコード生成を支援します。
>
> 従来のソフトウェア開発のプロセスでは、要件や設計の変更が頻繁に発生し、ドキュメントが最新の状態に保たれないことが多々ありました。そのため動くコードこそが唯一の真実であり、仕様であるという認識が一般的でした。
>
> 仕様駆動開発はこの関係を逆転させます。仕様書が唯一の真実であり、コードはその仕様を実装したものであるという考え方です。

### 2. Spec Kitのインストール

以下のコマンドで Spec Kit を使用してプロジェクトを初期化する

```bash
# Spec Kit のインストールには uv が必要
curl -LsSf https://astral.sh/uv/install.sh | sh

# Spec Kit を使用してプロジェクトを初期化する
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>

# 始めにどのコーディングエージェントを使用するかを選択する
# GitHub Copilot, Claude Code, Gemini CLI

# 以下は、コーディングエージェントを起動してからの作業
```

## 3. 公式サイトを読む

[github/spec-kit](https://github.com/github/spec-kit)

> 💫 Toolkit to help you get started with Spec-Driven Development
