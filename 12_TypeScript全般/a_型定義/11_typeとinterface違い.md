# TypeとInterfaceの違い

作成日 2021/05/12、更新日 2025/10/20

## 1. 解説記事を読む

解説記事 => [TypeScript の Interface と Type Alias の違い \- Qiita](https://qiita.com/sotszk/items/efe32e07e52dce329653)

- Typeは、再利用可能したい型の組み合わせに名前をつけるためのもの
- Interfaceは、クラスやオブジェクトを定義するためのもの

```javascript
type FeeClassification = {
  name: string;
  description: string;
  unitPrice: number;
  numOfPeople: number;
  totalPrice: number;
}

interface FeeClassification {
  name: string;
  description: string;
  unitPrice: number;
  numOfPeople: number;
  totalPrice: number;
}
```

MappedTypeというEnumっぽいものもある

```javascript
type Fruits = 'Apple' | 'Orange' | 'WaterMelon';
```

## 2. 自分なりのまとめ

- Typeは、TypeScriptが生まれた後に、すぐに生まれた補助役。型のまとめ役
- Interfaceは、インスタンス化できないクラス。C#からの継承

## 3. Interfaceは継承が可能

参照サイト => [インターフェースの継承](https://typescriptbook.jp/reference/object-oriented/interface/interface-inheritance)

継承で、プロパティを追加することが可能

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

継承で、ユニオン型から型を絞ることが可能

```javascript
interface Person {
  age: number | undefined;
}

interface Student extends Person {
  age: number;
}
```

## 4. 解説記事bを読む

[【結論】TypeScriptの型定義はtypeよりinterfaceを使うべき理由](https://zenn.dev/bmth/articles/interface-props-extends)

> typeの場合、合成された型であっても、最終的な構造が一目でわかります。私もこの「分かりやすさ」を理由に、業務では常にtypeを使うようにしていました。しかし、これが後に大きな問題を引き起こすことになるのです。
>
> type aliasが引き起こしたパフォーマンス地獄。プロジェクト全体の型定義を、機械的にtypeからinterfaceに一括置換する正規表現を書き、実行してみたら。あれだけ私たちを苦しめていたエディタの遅延はなくなり、ビルド時間は1分に戻ったのです。

- type: 即時評価。コンパイラはtypeを見つけるとその場で型を再帰的に解決・展開する
- interface: 遅延評価。コンパイラは型情報が必要になるまで計算を遅延させる

大規模なOSSライブラリのコードを見ると、そのほとんどがinterfaceで型定義されているのは、このスケーラビリティが理由です。

### 結論

オブジェクトの形状を定義する際は、まず`interface`を使う。`interface`で表現できない型（Union型など）を定義する必要がある場合に限り、`type`を使う。
