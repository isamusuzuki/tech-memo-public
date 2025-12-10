# ts-morph利用例

作成日 2025/12/10

 同作者による2つのプロジェクトを発見

## 1. suppress-ts-errors

解説記事: [@ts-expect-errorを自動追加！suppress-ts-errorsの紹介](https://zenn.dev/ryo_kawamata/articles/suppress-ts-errors)

使い方:

- `npx suppress-ts-errors` => 型エラーがでている箇所に`@ts-expoect-error`が追加される
- `npx suppress-ts-errors vue "src/**/*.vue` => VueのSFCのscript部分の型エラーにも対応

GitHub: [kawamataryo/suppress-ts-errors: CLI tool to add @ts-expect-errors to typescript type errors](https://github.com/kawamataryo/suppress-ts-errors)

## 2. vue-script-type-check

解説記事: [Vue.js SFC の script 部分のみを型チェックする CLI ツールを作ってみた](https://zenn.dev/ryo_kawamata/articles/vue-script-type-check)

使い方: `npx vue-script-type-check ./src/**/*.vue`

GitHub: [kawamataryo/vue-script-type-check: Command line Type-Checking tool for only the script part of Vue](https://github.com/kawamataryo/vue-script-type-check)
