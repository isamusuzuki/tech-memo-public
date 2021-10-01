# Vuex 4 と Composition API で双方向バインディングを実現する

作成日 2021/09/24

## 01. 参考記事を読む

参考記事 => [【Vue\.js】Vuex で双方向バインディング（v\-model を使いたい！） \- Qiita](https://qiita.com/key_it6/items/094cc773c2fdd1fc40a3)

参考記事は Vue.js 2 ベースの話であったが、`computed()` を、get と set に分ければ、`v-model` が使えようになるというポイントは、ためになった

## 02. サンプルコード

```text
--src/
    |--components/
    |   `--Keyword.vue  ... Vueコンポーネント + Composition API
    |--hooks/
    |   `--useKeyword.ts  ... computed()の中でgetとsetを用意する
    |--store/
    |   `--index.ts       ... Vuex 4で状態管理
    `--App.vue            ... 2つのコンポーネントを合体
```

src/components/Keyword.vue

```html
<template>
    <div class="field">
        <label class="label">キーワード</label>
        <div class="control">
            <input
                class="input"
                type="text"
                placeholder="キーワードの入力"
                v-model="keyword"
            />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { useKeyword } from '../hooks/useKeyword'

export default defineComponent({
    setup() {
        return {
            ...useKeyword(),
        }
    },
})
</script>
```

src/hooks/useKeyword.ts

```javascript
import { computed } from 'vue'
import { useStore } from 'vuex'
import { key } from '../store/index'

export const useKeyword = () => {
    const store = useStore(key)
    
    const keyword = computed({
        get: () => store.state.keyword,
        set: (val: string) => store.commit('setKeyword', val)
    })

    return {
        keyword
    }
}
```

src/store/index.ts

```javascript
import { InjectionKey } from 'vue'
import { createStore, Store } from 'vuex'

export interface State {
    keyword: string
}

export const key: InjectionKey<Store<State>> = Symbol()

export const store = createStore<State>({
    state: {
        keyword: ''
    },
    mutations: {
        setKeyword(state, val: string) {
            state.keyword = val
        }
    }
})
```
  
src/App.vue

```html
<template>
    <section class="section">
        <div class="container">
            <keyword></keyword>
        </div>
    </section>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Keyword from './components/Keyword.vue'

export default defineComponent({
    components: {
        Keyword,
    },
})
</script>
```
