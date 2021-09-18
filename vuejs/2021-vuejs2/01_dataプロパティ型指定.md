# data プロパティの型指定

作成日 2021/04/16

## トラブル発生

data プロパティで、初期値として、気軽に空の配列 `[]` を指定すると、TypeScript では NEVER 型となり、何を入れても赤線になってしまう。ちゃんと型を指定する必要がある

## 解決の糸口

- data プロパティに type を指定する

```javascript
type DataType = {
  key: number,
  value: string,
};

Vue.extend({
  data(): DataType {
    return {
      key: 12345,
      value: 'Suzuki',
    };
  },
});
```

## 実際のコード

- interface を書いて、それの配列とする

```javascript
import Vue from 'vue';

interface ICat {
  key: string;
  word: string;
  total: string;
  delimiter: string;
}

type DataType = {
  elements: Array<ICat>,
};

export default Vue.extends({
  date(): DataType {
    return {
      elements: [],
    };
  },
});
```
