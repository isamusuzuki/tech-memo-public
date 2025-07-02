# 音声生成API

作成日 2025/07/02

## 1. にじボイス

公式サイト => [にじボイス | AIによる感情豊かな音声生成サービス](https://nijivoice.com/)

- API利用 ... プリペイド式。1万文字あたり825円
- API仕様 ... [にじボイスAPI](https://docs.nijivoice.com/docs/getting-started)

## 2. VOICEVOX

公式サイト => [VOICEVOX | 無料のテキスト読み上げ・歌声合成ソフトウェア](https://voicevox.hiroshiba.jp/)

これはAPIではない。ローカルのアプリケーションである。これをMCPサーバーにして、AIアプリの中で使えるようにする記事があった

[VoiceVox MCP サーバー を試す](https://note.com/npaka/n/ne471d8a787dc)

> 「Claude Desktop」では、メニュー「Claude → 設定 → 開発者 → 構成を編集」の「claude_desktop_config.json」で以下のように設定します。

```json
{
  "mcpServers": {
    "voicevox": {
      "command": "uvx",
      "args": ["mcp-server-voicevox", "--voicevox-url=http://localhost:50021"]
    }
  }
}
```

50021ポートで待っているのが、Voice Vox MCPサーバーだと思った

[Sunwood-ai-labs/mcp-voicevox](https://github.com/Sunwood-ai-labs/mcp-voicevox/)

Python + uv + mcpパッケージ => MCPサーバーのかっこうの手本
