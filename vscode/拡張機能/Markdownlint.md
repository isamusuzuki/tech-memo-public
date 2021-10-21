# Markdownlint 機能拡張

作成日 2019/12/09

## 01. Markdownlint とは

ルールに基づいて Markdown 文を評価する

## 02. どのようなルールがあるのか

[markdownlint/Rules\.md at master · DavidAnson/markdownlint](https://github.com/DavidAnson/markdownlint/blob/master/doc/Rules.md)

よく注意されるルール

- MD001 - Heading levels should only increment by one level at a time
- MD009 - Trailing spaces
- MD033 - Inline HTML
- MD034 - Bare URL used
- MD036 - Emphasis used instead of a heading
- MD041 - First line in file should be a top level heading
- MD047 - Files should end with a single newline character

## 03. ルール適用を除外するには

除外したい Markdown ファイルに HTML コメントを追加する

```html
<!-- markdownlint-disable MD001 MD033 -->
```
