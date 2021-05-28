# Type と Interface の違い

作成日 2021/05/12、更新日 2021/05/28

## 解説記事を読む

解説記事 => [TypeScript の Interface と Type Alias の違い \- Qiita](https://qiita.com/sotszk/items/efe32e07e52dce329653)

- Type は、再利用可能したい型の組み合わせに名前をつけるためのもの
- Interface は、クラスやオブジェクトを定義するためのもの

```javascript
type FeeClassification = {
  name: string
  description: string
  unitPrice: number
  numOfPeople: number
  totalPrice: number
}

interface FeeClassification {
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

## 自分なりのまとめ

- Type は、TypeScript が生まれた後に、すぐに生まれた補助役。型のまとめ役
- Interface は、インスタンス化できないクラス。C# からの継承
