# Visual Studio Code を使いこなす

作成日 2020/01/29、更新日 2020/01/30

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

## HTML終了タグに自動でマルチカーソルを当てさせない

この機能はNovember 2019 (version 1.41)で導入された新機能で、HTML mirror cursorという

[Visual Studio Code November 2019](https://code.visualstudio.com/updates/v1_41)

> HTML mirror cursor in tags - Automatic multi-cursor in matching HTML tags.

左下歯車アイコン ＞ Settings ＞ "html mirror"を検索する
＞ HTML: Mirror Cursor On Matching Tag
＞ チェックボックスを外す

