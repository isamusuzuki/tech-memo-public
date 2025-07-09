# TypeScriptを構文解析する

作成日 2025/07/02

## 1. typescript-eslintを試す

[@typescript-eslint/typescript-estree](https://typescript-eslint.io/packages/typescript-estree/)

> The underlying code used by `@typescript-eslint/parser` that converts TypeScript source code into an ESTree-compatible form

```javascript
import { parse } from '@typescript-eslint/typescript-estree'

const ast = parse(code, {
    loc: true,
    range: false,
});
```

## 2. ASTの型定義を調べる

`node_modules/@typescript-eslint/types/dist/generated/ast-spec.d.ts`

- ArrowFunctionExpressionはbodyを持ち、それはBlockStatementかExpression
- BlockStatementはbodyを持ち、それはStatement[]
- IfStatementはalternateを持ち、それはStatement | null
- IfStatementはconsequentを持ち、それはStatement
- IfStatementはStatementである
- WhileStatement,WithStatement,DoWhileStatement,ForStatement,LabeledStatementはbodyを持ち、それはStatement
- CatchClause,FunctionExpressionはbodyを持ち、それはBlockStatement
- ExpressionStatementはbodyを持たない
