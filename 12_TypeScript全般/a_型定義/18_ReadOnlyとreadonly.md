# `ReadOnly<T>`型とreadonly修飾子

作成日 2025/11/10

## 1. `ReadOnly<T>`型とは

[Readonly&lt;T&gt; | TypeScript入門『サバイバルTypeScript』](https://typescriptbook.jp/reference/type-reuse/utility-types/readonly)

> `Readonly<T>`は、オブジェクトの型Tのプロパティをすべて読み取り専用にするユーティリティ型です。

```javascript
type Person = {
    surname: string;
    givenName: string;
}

// 下と同じ型になる
type ReadonlyPerson = ReadOnly<Person>;

type ReadonlyPerson = {
    readonly surname: string;
    readonly givenName: string;
}

// ReadOnlyの実装
type ReadOnly<T> = {
    readonly [P in keyof T]: T[P];
};
```

## 2. readonly修飾子とは

[オブジェクトの型のreadonlyプロパティ (readonly property) | TypeScript入門『サバイバルTypeScript』](https://typescriptbook.jp/reference/values-types-variables/object/readonly-property)

> TypeScriptでは、オブジェクトのプロパティを読み取り専用にすることができます。読み取り専用にしたいプロパティにはreadonly修飾子をつけます。読み取り専用のプロパティに値を代入しようとすると、TypeScriptコンパイラーが代入不可の旨を警告するようになります。
>
> readonlyはTypeScriptの型の世界だけの概念です。つまり、読み取り専用指定を受けたプロパティがチェックを受けるのはコンパイル時だけです。コンパイルされた後のJavaScriptとしては、readonlyがついていたプロパティも代入可能になります。
