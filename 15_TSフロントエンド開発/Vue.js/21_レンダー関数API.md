# [レンダー関数API](https://ja.vuejs.org/api/render-function)

作成日 2025/10/08

## 1. `h()`

仮想DOMノード(vnode)を作成します

- 第1引数 ... 文字列またはVueコンポーネント定義を指定する
- 第2引数 ... 渡されるprops
- 第3引数 ... 子要素

```javascript
import { h } from 'vue'

h('div', { id: 'foo' }, 'hello')
```
