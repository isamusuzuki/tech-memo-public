# Model Context Protocol (MCP)

作成日 2025/03/24、更新日 2025/03/28

## 1. MCPとは

[MCPに1mmだけ入門](https://zenn.dev/ks0318/articles/053b5bc1701c31)

> MCPとは？\
> AIエージェントと外部サービス間の通信手段のこと。\
> （怒られそうだが、AIエージェント用のAPIというのが実は一番伝わりやすいかも？）

- MCPホスト ... Cursor, Claude Desktopなどの、実際にユーザーが使うツール
- MCPクライアント ... Claude, GPT4などの、情報を取りに行くAIエージェント
- MCPサーバー ... Github、お天気サービスなどの、MCPクライアントに情報を渡す情報主

## 2. Figma MCP Server

ソースコード => [GLips/Figma-Context-MCP: MCP server to provide Figma layout information to AI coding agents like Cursor](https://github.com/GLips/Figma-Context-MCP)

紹介記事 => [【Cursor】FigmaにアクセスしてUIコードを自動生成！](https://zenn.dev/oke331/articles/97d5de75f06fb3)

## 3. playwright-mcp

ソースコード => [microsoft/playwright-mcp: Playwright Tools for MCP](https://github.com/microsoft/playwright-mcp)

> A Model Context Protocol (MCP) server that provides browser automation capabilities using Playwright.\
> This server enables LLMs to interact with web pages through structured accessibility snapshots,\
> bypassing the need for screenshots or visually-tuned models.

紹介記事 => [ClaudeでPlaywright MCPを使う（Windows）](https://zenn.dev/coco9122/articles/claude-playwright-mcp-coco9122)

> WindowsでClaude Desktopを使ってPlaywright MCPを使う際に少しハマったので、その解決方法をまとめます。
>
>- Claude Desktopのインストール
>- Playwright MCPのリポジトリのクローン
>- パッケージのインストール
>- Google Chromeの設定
>- Claude Desktopを開いて、MCPの設定
>- 実行
>
> 今回はせっかくになのでZennさん主催の第２回 AI Agent Hackathon with Google Cloudのページを見てもらうことにします。\
> プロンプトは以下のようにしました。

```text
browser_screenshotを用いて以下のURLのスクリーンショットをとってきてください
https://zenn.dev/hackathons/google-cloud-japan-ai-hackathon-vol2
```
