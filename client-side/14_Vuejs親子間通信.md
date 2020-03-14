# Vuejsで親子間通信を行う

作成日 2020/03/13

## $emitハンドラに引数を与える

子供のイベントで、親のイベントを呼び出すには `$emit()`を使う\
1番目の引数は、HTMLタグ上のv-bindディレクティブで、
親のイベントとバインドするための名前になる\
2番目以降の引数は、親のイベントに与える引数として使える

ドキュメント => [API — Vue\.js](https://jp.vuejs.org/v2/api/index.html) 「インスタンスメソッド/イベント」の項目に解説がある

### 子供側

JSファイル

```javascript
const itemRow = {
    props: ['item', 'index'],
    methods: {
        trigger(id) {
            this.$emit('child-event', id);
        }
    },
    template: `
    <tr>
        <td>{{item.name}}</td>
        <td><button v-on:click="trigger(item.id)"></td>
    </tr>
    `,
};
```

### 親側

htmlファイル

```html
<tr is="item-row" v-for="(item, index) in itemRows" 
    v-bind:item="item"
    v-bind:key="item.id" 
    v-bind:index="index" 
    v-on:child-event='listen'>
</tr>
```

JSファイル

```javascript
const app = new Vue({
    el: '#app',
    components: {
        'item-row': itemRow,
    },
    methods: {
        listen(id) {
            // 略
        }
    }    
});
```
