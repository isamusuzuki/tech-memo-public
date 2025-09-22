# esliint-plugiin-vue

作成日 2025/09/22

## 1. 公式サイトを読む

[Introduction | eslint-plugin-vue](https://eslint.vuejs.org/)

> Official ESLint plugin for Vue.js.
>
> This plugin allows us to check the `<template>` and `<script>` of `.vue` files with ESLint, as well as Vue code in `.js` files.
>
>- Finds syntax errors.
>- Finds the wrong use of Vue.js Directives.
>- Finds the violation for Vue.js Style Guide.
>
> ESLint editor integrations are useful to check your code in real-time.

## 2. Vue.js スタイルガイド

[Style Guide | Vue.js](https://vuejs.org/style-guide/)

- Priority A: Essential
- Priority B: Strongly Recommended
- Priority C: Recommended
- Priority D: Use with Caution

eslint-plugin-vueも、スタイルガイドに合わせた設定が用意されていた

- 'flat/essential'
- 'flat/strongly-recommended'
- 'flat/recommended'

ここでのflatとは、ESLint v9で採用されたフラットコンフィグのこと

## 4. 目にしたルールを調べる

いずれも`<template>`パートに関わるもの

Priority B: Strongly Recommended

- [vue/attribute-hyphenation](https://eslint.vuejs.org/rules/attribute-hyphenation.html)

- [vue/max-attributes-per-line](https://eslint.vuejs.org/rules/max-attributes-per-line.html)
- [vue/singleline-html-element-content-newline](https://eslint.vuejs.org/rules/singleline-html-element-content-newline.html)

Priority B: Strongly Recommended for Vue.js 3.x

- [vue/v-on-event-hyphenation](https://eslint.vuejs.org/rules/v-on-event-hyphenation.html)

Priority C: Recommended

- [vue/attributes-order](https://eslint.vuejs.org/rules/attributes-order.html)
