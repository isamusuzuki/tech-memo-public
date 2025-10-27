# avoidOptionals設定

作成日 2025/10/27

## 1. 解説記事aを読む

[[GraphQL Code Generator] より良い型を出力するために初期設定時に avoidOptionals を設定しよう](https://zenn.dev/koji_koji_koji/articles/4f5cc882716819)

> 自社でフロントエンドもバックエンドも開発してスキーマ駆動開発で進める場合は、設定してしておいた方がいいオプションがあります。それが、`avoidOptionals` です。
>
> 導入ガイドのドキュメント のコードそのままで avoidOptionals を設定しなかったり、`false` に設定すると、生成された型が optional になります。

```javascript
// avoidOptionals を設定しなかったり false にした場合
myField?: Maybe<string>

// avoidOptionals を true にした場合
myField: Maybe<string>
```

```javascript
import type { CodegenConfig } from '@graphql-codegen/cli'

const config: CodegenConfig = {
  // ...
  generates: {
    'path/to/file.ts': {
      plugins: ['typescript'],
      config: {
        // 追加
        avoidOptionals: true
      }
    }
  }
}
export default config
```

## 2. 解説記事bを読む

[GraphQL Code Generator(graphql-codegen) おすすめ設定 for TypeScript](https://zenn.dev/izumin/articles/ffc84c1b4310be)

> avoidOptionals: undefined を厳密に扱う
>
> nullable なフィールドが optional になるのを防ぐ。
> デフォルトの挙動だと`field?: Maybe<T>` だが、`avoidOptional` で`field: Maybe<T>` になる。
> 指定したフィールドは null だったら素直に null 返してくる（省略されない）のであれば、実際の挙動に沿った型にしておきたい、という意図。
>
> なお、GraphQL の引数は「値を入れない」と「null を渡す」を区別できるので、Input では `avoidOptionals` は無効化しておくほうがいい場合がある。

```javascript
import type { CodegenConfig } from '@graphql-codegen/cli'

const config: CodegenConfig = {
  generates: {
    'path/to/file.ts': {
      plugins: ['typescript'],
      config: {
        // 追加
        avoidOptionals: {
          field: true
          inputValue: false
          object: true
          defaultValue: false
        }
      }
    }
  }
}
export default config
```
