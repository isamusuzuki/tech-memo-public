# EditorConfigとは

作成日 2025/09/29

## 1. 公式サイト（英語）を読む

[EditorConfig](https://editorconfig.org/)

EditorConfigは、複数の開発者が様々なエディターやIDEを使って、同じプロジェクトに携わるときに、一貫したコーディングスタイルを維持できるようにお手伝いする

## 2. 解説記事を読む

[EditorConfigはESLint + Prettierと共存できる](https://zenn.dev/sumiren/articles/74f639f92e80b9)

> PrettierはEditorConfigを読み込み、フォーマットに反映している

Prettier公式ドキュメント => [Configuration File > EditorConfig](https://prettier.io/docs/configuration.html#editorconfig)

`.editorconfig`ファイルがプロジェクトにある場合は、Prettierは、このファイルをパースして、そのプロパティを関連するPrettiereの設定に変換する。この設定は`.prettierrc`ファイルによって上書きされる

## 3. `.editorconfig`ファイルの例

```bash
# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
insert_final_newline = true

# Matches multiple files with brace expansion notation
# Set default charset
[*.{js,py}]
charset = utf-8

# 4 space indentation
[*.py]
indent_style = space
indent_size = 4

# Tab indentation (no size specified)
[Makefile]
indent_style = tab

# Indentation override for all JS under lib directory
[lib/**.js]
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```
