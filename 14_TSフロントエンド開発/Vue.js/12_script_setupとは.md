# `<script setup>`とは

作成日 2025/01/06、更新日 2025/04/25

## 公式マニュアルを読む

[&lt;script setup&gt;](https://ja.vuejs.org/api/sfc-script-setup)

> `<script setup>`は単一ファイルコンポーネント（SFC）内で Composition API を使用するコンパイル時のシンタックスシュガー（糖衣構文）です。

### `defineProps()`と`defineEmits()`

> 完全な型推論のサポートつきで props と emits のようなオプションを宣言するために、`defineProps`と`defineEmits`の API を使用できます。これらは `<script setup>` の中で自動的に利用できるようになっています

「自動的に利用できる」=> import不要

```html
<script setup lang="ts">
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
</script>
```

> `defineProps`と`defineEmits`は、`<script setup>` 内でのみ使用可能なコンパイラーマクロです。インポートする必要はなく、`<script setup>`が処理されるときにコンパイルされます。
>
> `defineProps`は props オプションと同じ値を受け取り、`defineEmits` は emits オプションと同じ値を受け取ります。
>
> `defineProps`と`defineEmits`は、渡されたオプションに基づいて、適切な型の推論を行います。

### `defineModel()`

> このマクロは親コンポーネントから`v-model`経由で使用できる双方向バインディングの`props`を宣言するために使用できます。
> このマクロは内部でモデルの`props`と、それに対応する値更新イベントを宣言します。第一引数がリテラル文字列の場合、`props`の名前として使用されます。

```javascript
// 親からv-model:count経由で使用される、"count" propsをオプション付きで宣言する
const count = defineModel('count', { type: Number, default: 0 })

// defineModelも型引数を受け取ることができ、モデルの値や修飾子の型を指定できます:
const modelValue = defineModel<string>({ required: true })
```
