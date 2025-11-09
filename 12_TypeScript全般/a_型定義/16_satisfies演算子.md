# satisfies演算子

作成日 2025/09/17

## 解説記事を読む

[Type AnnotationsとSatisfies Operatorの違い](https://japanese-document.github.io/tips/2023/typescript-type-annotation-and-satisfies.html)

- 型アノテーションを使う ... 値の各プロパティの型は指定した型の対応するプロパティの型になる
- satisfies演算子を使う ... 変数の宣言時に変数に代入された値のプロパティの型になる

```javascript
type Foo = {
    bar: number | string
}

const foo1 = {
    bar: "1"
} satisfies Foo
// => foo1.barはstring型。現場優先

const foo2: Foo = {
    bar: "1"
}
// => foo2.barはnumber | string型。定義優先

foo1.bar = 2 // エラーになる
foo2.bar = 2 // エラーにならない
```

satisfies演算子は型を検証して実際にその型を満たしているかを確認し、かつ値の型推論結果を保持する
