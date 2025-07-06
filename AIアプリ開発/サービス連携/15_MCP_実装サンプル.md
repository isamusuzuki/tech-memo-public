# MCP 実装サンプル

作成日 2025/07/06

## 1. Pythonコード

[Pythonで理解するMCP](https://gihyo.jp/article/2025/06/monthly-python-2506)

- MCPホスト ... LLMアプリ本体。複数のクライアントインスタンスを生成・管理
- MCPクライアント ... ホストによって生成され、MCPサーバーに接続する
- MCPサーバー ... リソース、ツール、プロンプトを外部に公開

- リソース ... ユーザーやLLMが利用するコンテクストやデータ
- ツール ... LLMが呼び出す関数
- プロンプト ... 定義済みのプロンプトテンプレート

```text
--mcp-host-with-gradio/
    |--server/
    |   |--mcp_disk_usage.py ... MCPサーバーの実装
    |   `--mcp_os_name.py    ... MCPサーバーの実装
    `--app.py ... MCPホスト, MCPクライアントの実装
```

## 2. Node.jsコード

[リソースとプロンプトを提供する MCPサーバ の作成](https://note.com/npaka/n/nbf6347f9615b)
