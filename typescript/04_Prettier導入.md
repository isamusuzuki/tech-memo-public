# Prettier 導入

作成日 2021/02/14、更新日 2021/05/12

ファイルを保存する度に、スタイルを自動で修正するツール

公式サイト => [https://prettier.io/](https://prettier.io/)

インストール => `npm i -D prettier`

設定ファイルを用意することで、スタイルをカスタマイズすることができる

.prettierrc

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": true
}
```

Prettier は、JavaScript/TypeScript ファイルだけでなく、Markdown ファイルも自動で修正していく

Markdown ファイルの場合のみ、タブ幅を2文字に変更する方法

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": true,
  "overrides": [
    {
      "files": "*.md",
      "options": {
        "tabWidth": 2
      }
    }
  ]
}
```
