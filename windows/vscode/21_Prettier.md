## Prettier

作成日 2020/02/28、更新日 2020/04/02

## 01. Prettier とは

Prettier は、JavaScript などを修正・整形するフォーマッター

[Prettier \- Code formatter \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

## 02. Prettier をデフォルトのフォーマッターに指定する

.vscode/settings.json

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript, markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

### HTML ファイルだけは、Prettier に整形させない

```json
{
  "prettier.disableLanguages": ["html"],
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  }
}
```

## 03. Prettier の整形ルールをカスタマイズする

`.prettierrc`ファイル

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": true,
  "singleQuote": true
}
```

### 整形ルールの上書き

Markdown ファイルも Prettier が整形してくれるのは助かっているが、\
タブ幅がリストインデントに影響されて、4 文字になってしまうのに、困っていた\
ある条件の場合のみ、設定を上書きする方法を発見した

[Configuration File · Prettier](https://prettier.io/docs/en/configuration.html)

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": true,
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
