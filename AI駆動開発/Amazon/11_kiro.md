# kiro

作成日 2025/07/17

## 1. 解説記事を読む

[amazonの出したIDE「kiro」がめちゃくちゃ未来だったのでClaude Codeユーザーの人はみんな一度試してみてほしい](https://zenn.dev/sesere/articles/31d4b460c949e5)

- AmazonがVSCodeベースのIDEである「kiro」をリリースした
- コンセプトは Viable Code
- 「仕様書駆動開発」を明確に打ち出している
- AIに適切な指示を与えて適切な作業をしてもらうのは非常に難しい
- Amazonが考えた仕様書ファイルとそのフローが用意されている

1. Requirements 機能要件
2. Design 詳細設計
3. Task list 実装

モードは2種類。vibe = 今まで通り。spec = 仕様書駆動開発

kiroのデメリット

- devcontainerが使えない
- LLMモデルが弱い Claude Sonnet 4.0/3.7 用意されているのは2つだけ

## 2. 公式サイトを読む

[Kiro: The AI IDE for prototype to production](https://kiro.dev/)
