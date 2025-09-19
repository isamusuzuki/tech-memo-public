# Biomeとは

作成日 2025/09/19

## 1. 解説記事を読む

[Eslint+PrettierからBiomeへの移行](https://zenn.dev/ransakata/articles/e7972b77406545)

```bash
# インストール
npm install --save-dev --save-exact @biomejs/biome

# 設定ファイルの生成
npx @biomejs/biome init
# => biome.jsonが出来上がる

# ESLint, Prettierからの移行
npx @biomejs/biome migrate eslint --write
npx @biomejs/biome migrate prettier --write
# => biome.jsonが書き換わっている

# ./src配下のファイルを整形する
npx @biomejs/biome --write format ./src

# ./src配下のファイルをリントする
npx @biomejs/biome --write lint ./src
```

Biomeの特長

- 拡張やプラグインの追加インストールなし
- 実行速度が速い
- ドキュメントが手厚い

## 2. 公式サイトを読む

[Biome、Webのためのツールチェーン](https://biomejs.dev/ja/)

> フォーマット、リントなどが一瞬で完了します！\
> Prettierのようにコードをフォーマット、しかも高速

[はじめる | Biome](https://biomejs.dev/ja/guides/getting-started/)
