# Type と Interface の違い

作成日 2021/05/12、更新日 2025/04/21

## 1. 解説記事を読む

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

## 2. 自分なりのまとめ

- Type は、TypeScript が生まれた後に、すぐに生まれた補助役。型のまとめ役
- Interface は、インスタンス化できないクラス。C# からの継承

## 3. Interfaceは継承が可能

参照サイト => [インターフェースの継承](https://typescriptbook.jp/reference/object-oriented/interface/interface-inheritance)

プロパティを追加する

```javascript
interface Person {
  name: string;
  age: number;
}
 
interface Student extends Person {
  grade: number; // 学年
}
 
interface Teacher extends Person {
  students: Student[]; // 生徒
}
 
const studentA: Student = {
  name: "花子",
  age: 10,
  grade: 3,
};
 
const teacher: Teacher = {
  name: "太郎",
  age: 30,
  students: [studentA],
};
```

ユニオン型から選ぶ

```javascript
interface Person {
  age: number | undefined;
}
 
interface Student extends Person {
  age: number;
}
```
