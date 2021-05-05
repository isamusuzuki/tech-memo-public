# Prettier 導入

作成日 2021/02/14

ファイルを保存する度に、スタイルを自動で修正するツール

公式サイト => [https://prettier.io/](https://prettier.io/)

インストール `npm i -D prettier`

設定ファイルを用意することで、スタイルをカスタマイズすることができる

.prettierrc

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": true,
  "singleQuote": true
}
```

Prettier は、JavaScript ファイルだけでなく、Markdown ファイルも自動で修正していく

それを止めたい場合は、除外設定する

.prettierignore

```text
*.md
```
