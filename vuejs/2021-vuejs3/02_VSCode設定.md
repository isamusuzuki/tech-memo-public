# Visual Studio Code を設定する

作成日 2021/09/18、更新日 2021/10/11

## 01. JavaScript で `require()`を使っても、警告を表示させない

.vscode/settings.json

```json
{
  "javascript.validate.enable": false
}
```

## 02. ファイルごとにフォーマッターを指定する

.vscode/settings.json

```json
{
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

## 03. 拡張機能としてインストールした Prettier を設定する

.prettierrc

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
