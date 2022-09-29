# vue-clipboard3 を導入する

作成日 2021/10/06

## 01. vue-clipboard3 とは

[vue\-clipboard3 \- npm](https://www.npmjs.com/package/vue-clipboard3)

> Easily copy to clipboard in Vue 3 (composition-api)
>
> Thanks to vue-clipboard2 for inspiration!

vue-clipboard2 とは違う人が開発したようだが、ありがたいことに Composition API 対応となっている

## 02. 公式サイトのサンプルコード

```html
<template>
  <button @click="copy">Copy!</button>
</template>

<script lang="ts">
  import { defineComponent } from 'vue'
  import useClipboard from 'vue-clipboard3'

  export default defineComponent({
      setup() {
          const { toClipboard } useClipboard()
          const copy = async () => {
              await toClipboard('Any text you like')
          }

      }
      return { copy }
  })
</script>
```

## 03. 自分で書いたサンプルコード

src/components/Copy2Clip.vue

```html
<template>
  <span
    style="text-decoration: underline; cursor: pointer"
    @click="copy"
    v-text="target"
  ></span>
  <span v-if="showCopy">copy</span>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  import useCopy2Clip from '../composables/useCopy2Clip';

  export default defineComponent({
    props: {
      target: {
        type: String,
        required: true,
      },
    },
    setup(props) {
      return {
        ...useCopy2Clip(props.target),
      };
    },
  });
</script>
```

src/composables/useCopy2Clip.ts

```javascript
import { ref } from 'vue';
import useClipboard from 'vue-clipboard3';

export default (target: string) => {
  const showCopy = ref(false);
  const { toClipboard } = useClipboard();

  const copy = async () => {
    try {
      await toClipboard(target);
      showCopy.value = true;
      setTimeout(() => {
        showCopy.value = false;
      }, 300);
    } catch (e) {
      console.error(e);
    }
  };

  return {
    showCopy,
    copy,
  };
};
```
