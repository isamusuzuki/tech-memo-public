# VeeValidateとは

作成日 2025/10/06

## 1. 公式サイトを読む

[VeeValidate: Painless Vue.js forms](https://vee-validate.logaretm.com/v4/)

> VeeValidate is the most popular Vue.js form library. It takes care of value tracking, validation, errors, submissions and more.

## 2. Zodをバリデータとして使用できる

[Typed Schemas](https://vee-validate.logaretm.com/v4/guide/composition-api/typed-schema/)

> Zod is an excellent library for value validation which mirrors static typing APIs.
> You can use zod as a typed schema with the @vee-validate/zod package:

```bash
npm install @vee-validate/zod
```

## 3. Zodの前と後で紹介されているYup,Valibotとは？

[TypeScriptで使用するバリデーションライブラリ(yup,zod,valibot)の比較](https://qiita.com/hiros1991/items/e8da4cfd73c09f30f829)

今後の動向

- yup ... 最終アップデートが2023年で、特に今後のロードマップも見つからないため、あまり動きがない印象です。そのため、他のライブラリへの移行を検討した方が良いかもしれません。
- zod ... GitHubのスター数を見ると、現在は覇権を握っており、開発も活発に行われています。ただし、具体的なロードマップは見つかりませんでした。
- valibot ... 積極的に開発が行われている印象で、ロードマップも公式ブログで提示されています。今後もさらに便利になっていくことが期待されます。
