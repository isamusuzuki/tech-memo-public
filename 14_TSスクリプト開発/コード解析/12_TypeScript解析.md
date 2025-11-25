# TypeScriptを構文解析する

作成日 2025/07/02、更新日 2025/07/23

## 1. typescript-eslint/parserを試す

[@typescript-eslint/typescript-estree](https://typescript-eslint.io/packages/typescript-estree/)

> The underlying code used by `@typescript-eslint/parser` that converts TypeScript source code into an ESTree-compatible form

インストール => `bun add @typescript-eslint/typescript-estreer`

```javascript
import { parse } from '@typescript-eslint/typescript-estree'

// TypeScriptのASTを生成する
const ast = parse(code, {
    loc: true,
    range: false,
});
```

## 2. TypeScriptのASTの型定義を調べる

`node_modules/@typescript-eslint/types/dist/generated/ast-spec.d.ts`

## 3. 実体験をまとめる

### 3a. typeプロパティがDeclarationで終わるオブジェクト群

- "VariableDeclaration"は、declationsプロパティを持ち、値はオブジェクトの配列

### 3b. typeプロパティがDeclaratorで終わるオブジェクト群

- "VariableDeclarator"は、id,initプロパティを持ち、値はそれぞれオブジェクト

### 3c. typeプロパティがExpressionで終わるオブジェクト群

- "ArrowFunctionExpression"は、bodyプロパティを持ち、値はオブジェクト
- "AwaitExpression"は、argumentプロパティを持ち、値はオブジェクト
- "CallExpression"は、calleeプロパティを持ち、値はオブジェクト
- "MemberExpression"は、objectプロパティを持ち、値はオブジェクト

### 3d. typeプロパティがStatementで終わるオブジェクト群

- "BlockStatement"は、bodyプロパティを持ち、値はオブジェクトの配列
- "ExpressionStatement"は、expressionプロパティを持ち、値はオブジェクト
- "IfStatement"は、alternate,consequent,testプロパティを持ち、それぞれ値はオブジェクト
- "TryStatement"は、blockプロパティを持ち、値はオブジェクト
