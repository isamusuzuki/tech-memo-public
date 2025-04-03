# Model Context Protocol (MCP)

作成日 2025/03/24、更新日 2025/04/03

## 1. MCPとは

[MCPに1mmだけ入門](https://zenn.dev/ks0318/articles/053b5bc1701c31)

> MCPとは？\
> AIエージェントと外部サービス間の通信手段のこと。\
> （怒られそうだが、AIエージェント用のAPIというのが実は一番伝わりやすいかも？）

- MCPホスト ... Cursor, Claude Desktopなどの、実際にユーザーが使うツール
- MCPクライアント ... Claude, GPT4などの、情報を取りに行くAIエージェント
- MCPサーバー ... Github、お天気サービスなどの、MCPクライアントに情報を渡す情報主

[MCPはゲームチェンジャーになるのか](https://zenn.dev/eucyt/articles/mcp-server-impact)

> MCPが標準になりうる理由
>
> MCPはopenなprotocolであり、Anthropicという大手AI企業の強力な後押しがあります。

## 2. Figma MCP Server

ソースコード => [GLips/Figma-Context-MCP: MCP server to provide Figma layout information to AI coding agents like Cursor](https://github.com/GLips/Figma-Context-MCP)

紹介記事 => [【Cursor】FigmaにアクセスしてUIコードを自動生成！](https://zenn.dev/oke331/articles/97d5de75f06fb3)

紹介記事2 => [🚀 Figma MCP × Cursorで加速するUI実装とその先の工夫](https://zenn.dev/superstudio/articles/91ceb2f2f1d784)

> APIキーの共有問題
>
> Figma MCPを使用するためには、各開発者がFigmaのAPIキーを取得し、.cursor/mcp.jsonファイルに設定する必要があります。しかし、このAPIキーは個人単位で発行されるため、リポジトリにそのまま含めることはできません。また、mcp.jsonファイルは環境変数の読み込みに対応していないという制約もありました。
>
> 成功した点
>
>- **UI実装の高速化**: 単純なコンポーネントであれば、数分で実装が完了
>- **視覚的な正確さ**: 生成されたUIは、デザインとの視覚的な一致度が高い
>
> 実際の効果
>
> 正確な計測は行っていないですが、体感で約40%ほど作業時間が短縮されたと思います。

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

## 4. Model Context Protocol 標準化

[Model Context Protocol](https://github.com/modelcontextprotocol)

- [User Guide](https://modelcontextprotocol.io/introduction)
- [Specification](https://spec.modelcontextprotocol.io/specification/2025-03-26/)
- [Python SDK](https://github.com/modelcontextprotocol/python-sdk)
- [TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Java SDK](https://github.com/modelcontextprotocol/java-sdk)
