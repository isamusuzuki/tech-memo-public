# ts-loader 導入

作成日 2021/02/10

## 01. ts-loader とは

TypeScript を取り扱うローダー

[TypeStrong/ts\-loader: TypeScript loader for webpack](https://github.com/TypeStrong/ts-loader)

## 02. ts-loader を組み込む

必要なモジュールをインストールする

```bash
npm i -D typescript ts-loader

npx tsc --init
# => tsconfig.json が生成される
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.vue     // <= 一部書換
    |   |--index.ts        // <= 全面書換
    |   `--vue-shims.d.ts  // <= 追加
    |--package.json
    |--tsconfig.json       // <= 追加
    `--webpack.config.js   // <= 追記
```

src/components/App.vue

```html
<!-- // 以下を書き換える -->
<script lang="ts">
  import Vue from 'vue';

  export default Vue.extend({
    data() {
      return {
        name: 'Banana',
      };
    },
  });
</script>
```

src/index.ts

- JavaScript を TypeScript に書き換える
- 単一ファイルコンポーネントを読み込むときは `.vue` の拡張子まで書く

```javascript
import Vue from 'vue';
import App from './components/App.vue';

new Vue({
  el: '#app',
  components: { App },
  template: '<app/>',
});
```

src/vue-shims.d.ts

- 単一ファイルコンポーネントの型定義を TypeScript に教える

```javascript
declare module "*.vue" {
  import Vue from 'vue'
  export default Vue
}
```

tsconfig.json

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "es2015",
    "strict": true,
    "noImplicitReturns": true,
    "moduleResolution": "node"
  }
}
```

webpack.config.js

```javascript
module.exports = {
  module: {
    rules: [
      // 以下を書き加える
      {
        test: /\.ts$/,
        loader: 'ts-loader',
        exclude: /node_modules/,
        options: {
          appendTsSuffixTo: [/\.vue$/],
        },
      },
      // ここまで
    ],
  },
  resolve: {
    // '.ts'を書き加える
    extensions: ['.js', '.ts', '.vue'],
    alias: {
      vue$: 'vue/dist/vue.esm.js',
    },
  },
};
```

## 03. TypeScript 寄りのコードの書き方をする

必要なモジュールをインストールする

```bash
npm i -D vue-class-component vue-property-decorator
```

現在の構成に一部変更を加える

```text
--banana/
    |--dist/
    |   `--index.html
    |--src/
    |   |--components/
    |   |   `--App.ts      // <= 全面書換
    |   |--index.ts        // <= 一部書換
    |   `--vue-shims.d.ts
    |--package.json
    |--tsconfig.json       // <= 追記
    `--webpack.config.js
```

src/components/App.ts

```javascript
import Vue from 'vue';
import Component from 'vue-class-component';

@Component({
  template: `
    <div>
      <p class="title">Hello, {{ name }}!!!</p>
    </div>
  `,
})
export default class App extends Vue {
  name: string = 'Banana';
}
```

src/index.ts

```javascript
// 以下を書き換える
// import App from './components/App.vue';
import App from './components/App';
```

tsconfig.json

```json
{
  "compilerOptions": {
    // 以下を書き加える
    "experimentalDecorators": true
  }
}
```
