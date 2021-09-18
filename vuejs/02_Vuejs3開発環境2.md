# Vue.js 3 の開発環境を構築する その 2

作成日 2021/09/18

## 01. Visual Studio Code で気持ちよく開発するための設定

### JavaScript で `require()`を使っても、警告を表示させない

.vscode/settings.json

```json
{
  "javascript.validate.enable": false
}
```

### ファイルごとにフォーマッターを指定する

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
    "editor.defaultFormatter": "vscode.typescript-language-features"
  }
}
```

### 拡張機能でインストールした Prettier を設定する

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
