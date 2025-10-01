# `@ts-ignore`コメント

作成日 2025/09/09

## 1. `@ts-ignore`コメントとは？

次の行に発生するエラーを無視させる効力がある

## 2. ESLintが`@ts-ignore`コメントにエラーを出す

代わりに`@ts-expect-error`を使えと言う

[Use "@ts-expect-error" over "@ts-ignore" in TypeScript](https://dev.to/maafaishal/ts-expect-error-over-ts-ignore-in-typescript-5140)

## 3. `@ts-ignore`と`@ts-expect-error`の違い

`@ts-ignore`は、たとえエラーの原因を解決できたとしても、ずっと無視し続ける。コメントの消し忘れが発生しやすい

`@ts-expect-error`は、エラーが発生しない場合は、エラーを出す。コメントの消し忘れがない
