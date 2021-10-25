# TypeScript で Vue.js を開発するときにはまりやすいポイント

作成日 2021/05/03、更新日 2021/10/25

## 01. data に型をつける

数字や文字列、真偽といったプリミティブな型の場合は、初期値から正しく型推測される。オブジェクトの配列を使おうとして、初期値に空配列を与えると、never 型に型推測されて、それ以降、値を入れることができなくなる

```javascript
import Vue from 'vue'

export default Vue.extend({
    data() {
        return {
            tableShow: false, // => boolean型になる
            message: '', // => string型になる
            items: [] // => never型になる
    }
})
```

### 解決方法: DataType を定義する

```javascript
import Vue from 'vue'

interface IItem {
    id: number
    name: string
    success: boolean
}

type DataType = {
    tableShow: boolean
    message: string
    items: Array<IItem>
}

export default Vue.extend({
    data(): DataType {
        return {
            tableShow: false, // => boolean型になる
            message: '', // => string型になる
            items: [] // => IItemの配列になる
    }
})
```

## 02. props に型をつける

props に string の配列を与えると、各々は any 型になる。カッコ悪いやり方

```javascript
import Vue from 'vue';

export default Vue.extend({
  props: ['item', 'keyword', 'index'],
});
```

props に オブジェクトを与えると、型指定が可能になるが、小文字が大文字になっていることに注意。オブジェクトのプロパティの位置に、型は置けないため

```javascript
import Vue from 'vue';

export default Vue.extend({
  props: {
    keyword: String,
    index: Number,
  },
});
```

Object や Array の型をどうやって指定するか？

### 解決方法: PropType を導入する

```javascript
import Vue, { PropType } from 'vue'

interface IItem {
    id: number
    name: string
    success: boolean
}

export default Vue.extend({
    props: {
        item: Object as PropType<IItem>,
        keywords: Array as PropType<string[]>,
    },
});
```
