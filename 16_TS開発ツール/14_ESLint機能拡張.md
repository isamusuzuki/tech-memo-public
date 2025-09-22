# ESLint機能拡張

作成日 2025/09/22

## 1. 公式サイトを読む

[ESLint - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

この機能拡張は開いているワークスペースフォルダにインストールされているESLintライブラリを使用する。もし開いているフォルダになければ、グローバルにインストールされたバージョンを探す。もし、ESLintがローカルにもグローバルにもインストールされていない場合は、以下のコマンドを実行すべし。

```bash
npm install --save-dev eslint
```

新規プロジェクトの場合は、ESLintの設定ファイルを作成する必要があるだろう。以下のコマンドを実行することで設定ファイルの雛形を生成できる。

```bash
npx eslint --init
```

## 2. ESLintの設定ファイルと機能拡張の設定はどっちが優先される？

[Settings Options](https://github.com/Microsoft/vscode-eslint?tab=readme-ov-file#settings-options)

### `eslint.codeActionsOnSave.rules`

保存が実行された時のコードアクションで適用されるルールをコントロールする

```json
// セミコロンに関するルールだけ適用する
"eslint.codeActionsOnSave.rules": [
  "*semi*"
]
// TypeScriipt関連のルールを全て取り除き、それ以外のルールは適用する
"eslint.codeActionsOnSave.rules": [
  "!@typescript-eslint/*",
  "*"
]

// TypeScriptのセミコロンとインデントにい関するルールは適用し、それ以外のTypeScriptのルールは無視して、それ以外のルールは適用する
"eslint.codeActionsOnSave.rules": [
    "@typescript-eslint/semi",
    "@typescript-eslint/indent",
    "!@typescript-eslint/*",
    "*"
]
```

### `eslint.rules.customiizations`

該当プロジェクトの本当のESLintの設定と違う重大度を報告するようにルールを変更する

```json
// すべてのルールを警告に上書きする
"eslint.rules.customizations": [
  { "rule": "*", "severity": "warn" }
]

// no-なんちゃらのルールは情報レベルにして、それ以外のルールはダウングレードし、radixだけデフォルトに戻す
"eslint.rules.customizations": [
  { "rule": "no-*", "severity": "info" },
  { "rule": "!no-*", "severity": "downgrade" },
  { "rule": "radix", "severity": "default" }
]

// 自動整形可能なルールは情報レベルにする
"eslint.rules.customizations": [
  { "rule": "*", "fixable": true, "severity": "info" }
]
```
