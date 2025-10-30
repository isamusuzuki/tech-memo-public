# SFCとAI駆動開発は相性がいい

作成日 2025/10/30

## 解説記事を読む

同じ作者が元記事とその要約版を公開した

オリジナル => [Vue のカスタムブロックで「気軽に」始める仕様駆動開発のススメ - ANDPAD Tech Blog](https://tech.andpad.co.jp/entry/2025/10/22/100000)

ダイジェスト => [Vue SFC と AI 駆動開発の相性が良い理由](https://zenn.dev/ytr0903/articles/2a3cc9dfba9945)

> Vue の単一ファイルコンポーネント (SFC) は通常、 `<template>` `<script>` `<style>` で構成されますが、これ以外にも任意の名前でブロックを追加することができます。
>
> これがカスタムブロックであり、この仕様を利用するプラグインとして Vue I18n の `<i18n>` や、vite-plugin-vue-gql の `<gql>` などがあります。
>
> そしてこのカスタムブロックは当然ながらユーザーが自由に追加することもできます。そこで、このカスタムブロックとして `<spec>` ブロックを追加して、そこにコンポーネントの仕様を書く、というのが、今回提案する手法となります。

### 独自カスタムブロックを扱うための事前準備

[ツールガイド | Vuie.js > SFCカスタムブロック統合](https://ja.vuejs.org/guide/scaling-up/tooling#sfc-custom-block-integrations)

Viteを使用する場合、カスタムViteプラグインを使用する

```javascript
import vue from '@vitejs/plugin-vue'

const vueSpecPlugin = {
    name: 'vue-spec-plugin',
    transform(code, id) {
        if (/vue&type=spec/.test(id)) {
            return 'export default {}';
        }
        return;
    }
}

export default {
  plugins: [vue(), vueSpecPlugin],
}
```

### `<spec>`ブロック駆動開発

1. コンポーネントファイルを作成したら、script や template などは書かず、仕様だけを記述して保存
2. Copilotに「`<spec>`ブロックに従って、`src/components/SomeComponent.vue`を実装してください」と伝える
3. [Vue SFC Playground](https://play.vuejs.org/)にコピーして動かしてみる
4. 更新するときは、`<spec>`ブロックもセットで指示を出す

### `<spec>`ブロックを活用するメリット

- 手軽で、段階的に導入しやすい
- `<spec lang="md">`と設定すれば、Markdownで書ける
- 仕様と実装が1つの場所にまとまる
- 仕様がコンテキストとしてAIに必ず提供される
