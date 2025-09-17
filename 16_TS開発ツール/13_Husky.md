# Husky

Gitフックを利用してコード整形・自動テストを実現するツール

作成日 2025/09/17

## 1. 公式サイトを読む

[Husky](https://typicode.github.io/husky/)

インストール => `npm install --save-dev husky`

```bash
# 初回設定
npx husky init
# .husky/フォルダにpre-commitスクリプトを生成
# package.jsonのprepareスクリプトを更新
```

## 2. 解説記事を読む

[Husky 入門：Node.js プロジェクトに Git Hooks を導入して、コミット品質を自動で守る](https://qiita.com/oharu121/items/2a38db1c76376de63cdb)

> prepareスクリプト: npm install時に自動でhusky installを実行
> チームメンバーがnpm installするだけでHuskyが有効になる

## 3. Gitフックとは

[Git - Git フック](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%83%95%E3%83%83%E3%82%AF
)

> フックは、Gitディレクトリの hooks サブディレクトリ（一般的なプロジェクトでは、.git/hooks ）に格納されています。
