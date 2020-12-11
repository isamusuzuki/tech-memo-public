# VuePress とは

作成日 2019/04/16

Vue.js の作者が開発している静的サイトジェネレーター。Markdown ファイルを変換する。Vue.js の知識が必要になる場面は今のところない

公式サイト => [https://v1.vuepress.vuejs.org/](https://v1.vuepress.vuejs.org/)

公式ガイド => [https://v1.vuepress.vuejs.org/guide/#how-it-works](https://v1.vuepress.vuejs.org/guide/#how-it-works)

## 01. VuePress の参考記事

[VuePress をお試ししてみた](https://qiita.com/dojineko/items/aae7e6d13479e08d49fd)

[検索機能などをサポートした静的サイトジェネレーター「VuePress」を試した](https://kakakakakku.hatenablog.com/entry/2019/04/03/193851)

## 02. VuePress を試してみる

npm よりも yarn の利用が推奨される

```bash
mkdir banana
cd banana
yarn init -y
yarn add vuepress@next -D
mkdir docs
vi docs/index.md
```

package.json

```json
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```

yarn もスクリプトに対応していた。`yarn docs:dev`でスクリプトが起動する

docs/.vuepress/config.js

```js
module.exports = {
  title: '技術メモ',
  description: '技術メモ',
  themeConfig: {
    nav: [{ text: 'Top', link: '/' }],
    sidebar: [
      {
        title: 'IoT',
        collapsable: true,
        children: [['/jasper', 'Cisco Jasperとは？']],
      },
      {
        title: 'モバイルネットワーク',
        collapsable: true,
        children: [['/jasper', 'Cisco Jasperとは？']],
      },
    ],
  },
};
```

### VuePress を試した感想

![vuepress-screenshot.png](https://imgur.com/4ZxEzks.png)

- 日本語の検索が最初から効いている
- 検索する範囲は H2 までと決まっている
- モバイル対応している

MkDocs と比べると、コンパイルに時間がかかるのが唯一のデメリットか？

## 03. GitLab Pages に展開するには

CI 設定ファイルをひとつ追加するだけ

[GitLab Pages examples / vuepress · GitLab](https://gitlab.com/pages/vuepress)

.gitlab-ci.yml の例

```yml
image: node:9.11.1
pages:
  cache:
    paths:
      - node_modules/
  script:
    - yarn install
    - yarn build
  artifacts:
    paths:
      - public
  only:
    - master
```
