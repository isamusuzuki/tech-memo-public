# Biomeとは

作成日 2025/09/19、更新日 2025/09/22

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

## 3. Vueのサポート状況

[HTML拡張言語のサポート](https://biomejs.dev/ja/internals/language-support/#html%E6%8B%A1%E5%BC%B5%E8%A8%80%E8%AA%9E%E3%81%AE%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88)

`Vue`のサポート状況は「一部サポート」でいくつか注意ありとなっている

> `.vue` ファイルでは、`<script>`タグ部分のみがサポートされています。
>
> `.vue` ファイルをフォーマットする際、JavaScript/TypeScriptコードのインデントは最初から始まります。
>
> `.vue` ファイルを静的解析する際、コンパイラエラーを防ぐためにいくつかのルールをオフにすることをお勧めします。
