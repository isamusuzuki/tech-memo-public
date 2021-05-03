# 独自のstore（状態管理）を導入

作成日 2021/04/17

参考記事 => [TypeScript で Vuex を使わないという選択肢 \- Qiita](https://qiita.com/hiroiku/items/e6b3a5c823c367c8e45b)

> Vuex を使わないことのメリット
>
> Vue 2.6.0 から登場した `Vue.observable()` により簡単にリアクティブなオブジェクトを作ることができるようになりました。

## `Vue.observable()` とは

> オブジェクトをリアクティブにします。内部的には、Vue は data 関数から返されたオブジェクトに対してこれを使っています。
>
> 戻り値のオブジェクトは、描画関数 や 算出プロパティ の中で直接使え、値が変更されたときには適切な更新をトリガーします。単純なシナリオでは、コンポーネントをまたぐ最小の state ストアとして使用することもできます

## サンプルコードを書いてみる

```text
--src/testman/
    |--components/
    |   `--Modal.vue
    |--store/
    |   `--index.ts
    `--App.vue
```

store/index.ts

```javascript
import Vue from 'vue'

class Store {
    private _modalActive = false

    public get modalActive() {
        return this._modalActive
    }

    public openModal() {
        this._modalActive = true
    }

    public closeModal() {
        this._modalActive = false
    }
}

export default Vue.observable(new Store())
```

App.vue

- 親コンポーネントから状態を変更できる

```html
<script lang="ts">
import Vue from 'vue';
import Modal from './components/Modal.vue';
import store from './store/index';

export default Vue.extend({
    components: {
        modal: Modal,
    },
    methods: {
        showModal() {
            store.openModal();
        },
    },
});
</script>
```

Modal.vue

- 子コンポーネントから状態を取得できる
- 子コンポーネントから状態を変更できる

```html
<script lang="ts">
import Vue from 'vue';
import store from '../store/index';

export default Vue.extend({
    computed: {
        active() {
            return store.modalActive;
        },
    },
    methods: {
        close() {
            store.closeModal();
        },
    },
});
</script>
```
