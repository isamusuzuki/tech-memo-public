# ESLint導入

作成日 2025/09/16

## 1. 公式サイト（英語）を読む

[Find and fix problems in your JavaScript code - ESLint - Pluggable JavaScript Linter](https://eslint.org/)

[Getting Started with ESLint - ESLint - Pluggable JavaScript Linter](https://eslint.org/docs/latest/use/getting-started)

## 2. Vite+VueのプロジェクトにESLintをインストールしてみる

```bash
# すでにpackage.jsonがあるフォルダで実行する
npm init @eslint/config@latest
# √ What do you want to lint? · javascript
# √ How would you like to use ESLint? · problems
# √ What type of modules does your project use? · esm
# √ Which framework does your project use? · vue
# √ Does your project use TypeScript? · Yes
# √ Where does your code run? · browser
# √ Which language do you want your configuration file be written in? · ts
# √ Would you like to add Jiti as a devDependency? · Yes
# √ Would you like to install them now? · Yes
# √ Which package manager do you want to use? · npm
```

- eslint.config.tsファイルが出来る（v9から導入された新しい設定ファイル）
- .eslintrcファイルはレガシーとなった（8.57未満の古いバージョン用）
- Jitiは、Node.js上でTypeScriptを直接実行するユーティリティ

```javascript
import js from "@eslint/js";
import globals from "globals";
import tseslint from "typescript-eslint";
import pluginVue from "eslint-plugin-vue";
import { defineConfig } from "eslint/config";

export default defineConfig([
  { files: ["**/*.{js,mjs,cjs,ts,mts,cts,vue}"], plugins: { js }, extends: ["js/recommended"], languageOptions: { globals: globals.browser } },
  tseslint.configs.recommended,
  pluginVue.configs["flat/essential"],
  { files: ["**/*.vue"], languageOptions: { parserOptions: { parser: tseslint.parser } } },
]);
```

`pluginVue.configs["flat/essential"],`を`pluginVue.configs["flat/recomended"],`に変更したら、
警告が出るようになった

```bash
npx eslint src/App.vue

C:\Users\{username}\workspaces\vite-vue-dojo\app\src\App.vue
   7:32  warning  'target' should be on a new line                      vue/max-attributes-per-line
   8:28  warning  'class' should be on a new line                       vue/max-attributes-per-line
   8:41  warning  'alt' should be on a new line                         vue/max-attributes-per-line
   8:57  warning  Disallow self-closing on HTML void elements (<img/>)  vue/html-self-closing
  10:34  warning  'target' should be on a new line                      vue/max-attributes-per-line
  11:35  warning  'class' should be on a new line                       vue/max-attributes-per-line
  11:52  warning  'alt' should be on a new line                         vue/max-attributes-per-line
  11:67  warning  Disallow self-closing on HTML void elements (<img/>)  vue/html-self-closing
  14:15  warning  Attribute 'msgOptin' must be hyphenated               vue/attribute-hyphenation

✖ 9 problems (0 errors, 9 warnings)
  0 errors and 7 warnings potentially fixable with the `--fix` option.
```

## 3. eslint-plugin-vue

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

## 4. Vue.jsスタイルガイド

Vue.jsにはスタイルガイドが定められていた

[Style Guide | Vue.js](https://vuejs.org/style-guide/)

- Priority A: Essential
- Priority B: Strongly Recommended
- Priority C: Recommended
- Priority D: Use with Caution
