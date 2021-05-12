# Interface と Type Alias の違い

作成日 2021/05/12

解説記事 => [TypeScript の Interface と Type Alias の違い \- Qiita](https://qiita.com/sotszk/items/efe32e07e52dce329653)

解説記事を読んで、自分なりにまとめた

- interface は、クラスやオブジェクトを定義するためのもの
- TypeAlias は、再利用可能したい型の組み合わせに名前をつけるためのもの

```javascript
interface FeeClassification {
  name: string
  description: string
  unitPrice: number
  numOfPeople: number
  totalPrice: number
}

type FeeClassification = {
  name: string
  description: string
  unitPrice: number
  numOfPeople: number
  totalPrice: number
}
```

MappedType という Enum っぽいものもある

```javascript
type Fruits = 'Apple' | 'Orange' | 'WaterMelon';
```
