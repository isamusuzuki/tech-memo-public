# Codex CLI

作成日 2025/04/23

## Code CLIとは

参照サイト => [OpenAI Dev Digest まとめ（o3, o4-mini, GPT-4.1, Codex など）](https://zenn.dev/openaidevs/articles/2690ad6dd593ea)

> 自然言語で指示をするだけで、ローカルのプロジェクトの開発作業を行うことができる、オープンソースのローカルコーディングエージェント Codex CLI をリリースしました（米国時間 4/16 発表）。

デモ・ビデオ => [OpenAI Codex CLI](https://www.youtube.com/watch?v=FUq9qRwrDrI)

ユーザーガイド（英語） => [OpenAI Codex CLI – Getting Started](https://help.openai.com/en/articles/11096431-openai-codex-cli-getting-started)

Quick Start

```bash
# Install:
npm install -g @openai/codex

# Authenticate:
export OPEN_API_KEY="<OAI KEY>"

# Run in Sugggest mode:
# プロジェクトのルートで、codexとタイプして命令する
"Explain this repo to me."

# Review outputs:
# Codexがパッチやコマンドを出力するので、承認・拒否・修正を行う
```
