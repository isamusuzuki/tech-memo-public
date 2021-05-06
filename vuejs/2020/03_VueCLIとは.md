# Vue CLI とは

作成日 2020/11/10

## 01. 事の発端

### Nuxt.js と Vue CLI 3 を比較する記事を見つけた

[NUXT いるのかどうか \(Vue CLI 3 との比較\) \- Qiita](https://qiita.com/macoshita/items/bf295a1e0f5fefff3d8e)

> Vue を使うときの選択肢\
> 現状、 Vue CLI 3 と NUXT の二択だと思って良い。
>
> Vue CLI 3 とはなにをするものなのか\
> Vue のツールチェーンがほぼ全部入りした CUI/GUI ツール。

### Nuxt.js いらない説

[Nuxt\.js いらない説 \- GA ミント至上主義](https://uyamazak.hatenablog.com/entry/2018/08/15/124952)

> まず Vue CLI 3 で始めた方がいい。最初から大人数で大きなアプリを作るつもりだったり、SSR が必要だったら Nuxt でいいかも。
>
> Vue は必要に応じてパーツを追加、拡張していこう、みたいな考えを貫いており、コア機能をシンプルに保っています。
>
> 一方 Nuxt.js は Rails みたいにいきなりどかっとサーバー環境まるごと全部入り環境な感じなので、Nuxt で一通り用意されてる構造や規約を理解して、それに従って作っていく感じになります。

## 02. 公式サイトを読む

[Vue CLI](https://cli.vuejs.org/)

> Standard Tooling for Vue.js Development

公式ドキュメント => [Overview \| Vue CLI](https://cli.vuejs.org/guide/)

> The CLI (`@vue/cli`) is a globally installed npm package and provides the `vue` command in your terminal.

### Ubuntu に Vue CLI をインストールする

[Installation \| Vue CLI](https://cli.vuejs.org/guide/installation.html)

```bash
sudo npm install -g @vue/cli

vue --version
# @vue/cli 4.5.8

npm list -g --depth=0
# /usr/lib
# ├── @vue/cli@4.5.8
# └── npm@6.14.8

# アップグレードするときは
sudo npm upgrade -g @vue/cli
```

### インスタントプロトタイピングとは

[Instant Prototyping \| Vue CLI](https://cli.vuejs.org/guide/prototyping.html)

> You can rapidly prototype with just a single \*.vue file with the vue serve and vue build commands, but they require a global addon to be installed along with the Vue CLI.

```bash
sudo npm install -g @vue/cli-service-global

mkdir avocado
cd avocado
touch App.vue
vi App.vue
vue serve
#  App running at:
#  - Local:   http://localhost:8080/
#  - Network: http://192.168.1.13:8080/
```

App.vue

```html
<template>
  <h1>Hello!</h1>
</template>
```

`vue serve`コマンドは、カレントディレクトリからエントリーファイルを自動的に見つける\
以下のファイルがデフォルトである

- main.js
- index.js
- App.vue
- app.vue

```bash
vue build
```

出力先のディレクトリは、デフォルトで`dist`となっている

```text
--avocado/
    |--App.vue
    |--dist/
    |   |--index.html
    |   `--js/
    |       |--app.ff6863e9.js
    |       |--app.ff6863e9.js.map
    |       |--chunk-vendors.1fdd4eef.js
    |       `--chunk-vendors.1fdd4eef.js.map
    `--node_modules
```

### プロジェクトを新規作成する

[Creating a Project \| Vue CLI](https://cli.vuejs.org/guide/creating-a-project.html)

```bash
# 新しいプロジェクトをつくる
vue create banana
# ? Please pick a preset: (Use arrow keys)
#❯ Default ([Vue 2] babel, eslint)
#  Default (Vue 3 Preview) ([Vue 3] babel, eslint)
#  Manually select features

# マニュアル選択に入ると、以下のメッセージが登場する

# ? Check the features needed for your project: (Press <space> to select, <a> to t
# oggle all, <i> to invert selection)
# ❯◉ Choose Vue version
#  ◉ Babel
#  ◯ TypeScript
#  ◯ Progressive Web App (PWA) Support
#  ◯ Router
#  ◯ Vuex
#  ◯ CSS Pre-processors
#  ◉ Linter / Formatter
#  ◯ Unit Testing
#  ◯ E2E Testing
```
