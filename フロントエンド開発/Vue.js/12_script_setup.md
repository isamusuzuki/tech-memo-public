# `<script setup>`

作成日 2025/01/06

公式マニュアル => [&lt;script setup&gt;](https://ja.vuejs.org/api/sfc-script-setup)

> `<script setup>`は単一ファイルコンポーネント（SFC）内で Composition API を使用するコンパイル時のシンタックスシュガー（糖衣構文）です。

## `defineProps()`と`defineEmits()`

> 完全な型推論のサポートつきで props と emits のようなオプションを宣言するために、`defineProps`と`defineEmits`の API を使用できます。これらは `<script setup>` の中で自動的に利用できるようになっています

```html
<script setup>
const props = defineProps({
  foo: String
})

const emit = defineEmits(['change', 'delete'])
// セットアップのコード
</script>
```

> `defineProps`と`defineEmits`は、`<script setup>` 内でのみ使用可能なコンパイラーマクロです。インポートする必要はなく、`<script setup>`が処理されるときにコンパイルされます。
>
> `defineProps`は props オプションと同じ値を受け取り、`defineEmits` は emits オプションと同じ値を受け取ります。
>
> `defineProps`と`defineEmits`は、渡されたオプションに基づいて、適切な型の推論を行います。
