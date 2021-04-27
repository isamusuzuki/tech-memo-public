# PropType 導入

作成日 2021/04/20

PropTypeは、ArrayやObjectに型の指定をするときに使う

```javascript
import Vue, {PropType} from 'vue'

export default Vue.extend({
    props: {
        onBoard : {type: Array as PropType<String[][]>},
        inBlackHand: {type: String},
        inWhiteHand: {type: String}
    }
})
```

参照記事 => [Vue \+ TypeScriptでpropsのObjectやArrayに型をつける \- Qiita](https://qiita.com/iMasanari/items/31d8a26c7ee22793585c)
