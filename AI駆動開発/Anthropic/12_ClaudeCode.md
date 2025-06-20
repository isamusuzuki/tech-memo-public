# Claude Code

作成日 2025/03/11、更新日 2025/06/20

## 1. 公式ガイド（日本語）を読む

[Claude Code 概要](https://docs.anthropic.com/ja/docs/agents-and-tools/claude-code/overview)

> Claude Code は、ターミナルに常駐し、コードベースを理解し、自然言語コマンドを通じてより速くコーディングを支援するエージェント型コーディングツールです。
>
> Claude Code の主な機能は以下の通りです：
>
> - コードベース全体でのファイル編集とバグ修正
> - コードのアーキテクチャとロジックに関する質問への回答
> - テスト、リンティング、その他のコマンドの実行と修正
> - git の履歴検索、マージコンフリクトの解決、コミットと PR の作成

## 2. 参照サイトを読む

[Claude Code の使い方](https://note.com/npaka/n/n3d754c78f439)

> 「Claude Code」はターミナルで直接動作し、プロジェクトのコンテキストを理解して実際のアクションを実行します。
> コンテキストにファイルを手動で追加する必要はありません。「Claude」は必要に応じてコードベースを探索します。

## 3. 参照サイトを読むその2

[Mac環境で手を動かしながらClaude Codeを学ぶ](https://zenn.dev/fendo181/articles/1c859828fa2e17)

```bash
# インストール
npm install -g @anthropic-ai/claude-code

# バージョン確認
claude-code --version

# セットアップを行う
claude
# - 画面のモード選択
# - プランの選択
# - ターミナル設定
# - カレントディレクトリへのアクセス許可
```

claudeコマンドを実行すると、入力ボックスが登場する。以後はその中で使えるコマンドの一部

```text
/help
/init ... 設定ファイルの生成
/cost .. 使用料金の確認
```
