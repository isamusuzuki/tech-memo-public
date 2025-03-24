# Model Context Protocol (MCP)

作成日 2025/03/24

## 1. MCPとは

[MCPに1mmだけ入門](https://zenn.dev/ks0318/articles/053b5bc1701c31)

> MCPとは？\
> AIエージェントと外部サービス間の通信手段のこと。\
> （怒られそうだが、AIエージェント用のAPIというのが実は一番伝わりやすいかも？）

- MCPホスト ... Cursor, Claude Desktopなどの、実際にユーザーが使うツール
- MCPクライアント ... Claude, GPT4などの、情報を取りに行くAIエージェント
- MCPサーバー ... Github、お天気サービスなどの、MCPクライアントに情報を渡す情報主

## 2. Figma MCP Server

[Cursor】FigmaにアクセスしてUIコードを自動生成！](https://zenn.dev/oke331/articles/97d5de75f06fb3)

[GLips/Figma-Context-MCP: MCP server to provide Figma layout information to AI coding agents like Cursor](https://github.com/GLips/Figma-Context-MCP)
