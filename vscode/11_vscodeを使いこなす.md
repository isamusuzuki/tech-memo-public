# Visual Studio Code をつかいこなす

作成日 2020/01/29

## HTML ファイルは Prettier にフォーマットさせない

.vscode/settings.json

```json
{
  "prettier.disableLanguages": ["html"],
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  }
}
```
