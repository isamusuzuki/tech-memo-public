# Vuejsで親子間通信を行う

作成日 2020/03/13

## $emitハンドラに引数を与える

子供のイベントで、親のイベントを呼び出すためには `$emit()`を使う\
1番目の引数は、v-bindディレクティブで親のイベントとバインドするための名前になるが、\
2番目の引数は、親のイベントに与える引数として使える\
もし複数の引数を与えたいときは、2番目の引数をオブジェクトにすればいい


### 子供側

JSファイル

```javascript
const itemRow = {
    props: ['item', 'index'],
    methods: {
        throw(id) {
            this.$emit('child-event', id);
        }
    },
    template: `
    <tr>
        <td>{{item.name}}</td>
        <td><button v-on:click="throw(item.id)"></td>
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
    v-on:child-event='catch'>
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
        catch(id) {
            // 略
        }
    }    
});
```
