# TypeScript 上で Vuex を導入する

作成日 2021/04/17

## 問題発生

コンポーネントで、Vuex ストアにアクセスするために、`this.$store` を書いたら、赤線が出てしまう

## 解決の糸口

公式記事 => [TypeScript Support \| Vuex](https://next.vuex.vuejs.org/guide/typescript-support.html)

> Vuex doesn't provide typings for `this.$store` property out of the box. When used with TypeScript, you must declare your own module augmentation.

そこは自分で定義しないといけない領域だった

Vue3 + Vuex4の時代になれば、TypeScriptとの親和性が高くなってくるが、現時点 (Vue2 + Vuex3) では、とても無理だと思った
