# Gemini CLI

作成日 2025/06/26

## 1. Gemini CLIがリリースされた

オープンソースのAIエージェント

ソースコード => [google-gemini/gemini-cli: An open-source AI agent that brings the power of Gemini directly into your terminal.](https://github.com/google-gemini/gemini-cli/)

前提条件：Node.js v18以上がインストールされていること

```bash
npm install -g @google/gemini-cli
gemini
```

リミットを増やす方法

[Google AI Studio](https://aistudio.google.com/apikey)でキーを生成する

環境変数にキーを登録する

```bash
export GEMINI_API_KEY="YOUR_API_KEY"
```

## 2. 参考記事を読む

[Gemini CLIをWindowsで使ってみた](https://zenn.dev/acntechjp/articles/ac9b77b935e696)

AnthropicのClaude Codeや、OpenAIのCodex CLIは、Windows 11で使用する場合WSL上で動かす必要があった。一方で、Gemini CLIはWSLがなくても直接PowerShell上で動く

無料枠あり

- 個人のGoogleアカウントでログインするだけで無料で利用可能
- 業界最大級の利用枠：1分間に60リクエスト、1日1,000リクエストまで無料
- 通常は有料のGemini Code Assistライセンスも無料で付与

Gemini Code Assistとの連携

- エージェントモードを通じて、複数ステップの計画立案や自動エラー回復などの高度な機能を利用可能
