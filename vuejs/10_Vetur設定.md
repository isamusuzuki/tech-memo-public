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

### Vetur スニペットを追加する

.vscode/vetur/snippets/composables.ts

```javascript
import { computed, onMounted, reactive, ref } from 'vue'
import { useStore } from 'vuex'
import { useRoute, useRouter } from 'vue-router'
import { key } from './store'

export default () => {
    const store = useStore(key)
    const route = useRoute()
    const router = useRouter()

    return {
    }
}
```

.vscode/vetur/snippets/router.ts

```javascript
import { createRouter, createWebHashHistory } from 'vue-router'

export const routes = [
    { path: '/', name: 'index', component: null },
    { path: '/about', name: 'about', component: null },
]

export default createRouter({
    history: createWebHashHistory(),
    routes
})
```

.vscode/vetur/snippets/

```javascript
import axios from 'axios'
import { InjectionKey } from 'vue'
import { createStore, Store } from 'vuex'

export interface State {
}

export const key: InjectionKey<Store<State>> = Symbol()

export const store = createStore<State>({
    state: {
    },
    mutations: {
    },
    actions: {
    }
})
```
