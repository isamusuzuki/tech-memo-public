# Vetur を設定する

作成日 2021/10/11

## 01. Vetur のスニペット機能をカスタマイズする

### Vetur スニペットの問題点

- 拡張子が `.vue` のファイルにおいて、`typescript` とタイプすると、以下のスニペットを貼り付けられる
- 残念ながらバージョン 2 系の書式である。これをバージョン 3 系に入れ替えたいと思った

```html
<script lang="ts">
  import Vue from 'vue';
  export default Vue.extend({});
</script>
```

### Vetur スニペットのカスタマイズ方法

公式ドキュメント => [Snippet \| Vetur](https://vuejs.github.io/vetur/guide/snippet.html)

```text
--PROJECT-NAME/
    `--.vscode/
        `--vetur/
            `--snippets/
                `--typescript.vue
```

typescript.vue

```html
<script lang="ts">
  import { defineComponent } from 'vue';

  export default defineComponent({
    components: {},
    setup() {
      return {};
    },
  });
</script>
```

- Vetur に、このファイルを覚えさせるために、Visual Studio Code の再起動が必要
- `typescript` とタイプして、このスニペットが登場するのは、スニペットが「ファイル名検索」でもあるから
- プロジェクトレベルで登録されたスニペットには鞄のアイコンがつく。それで見分けられる
