# TypeScriptを直接実行する

作成日 2025/04/25

## 1. tsx

tsxを使うと、Typescriptを直接実行できる

インストール => `npm i -D tsx`

公式ホームページ（英語） => [TypeScript Execute (tsx) | tsx](https://tsx.is/)

>- Seamless TypeScript execution
>- Seamless CJS ↔ ESM imports

## 2. 具体的な使い方

```text
--some_project/
    |--dist/
    |   `--index.js ... 変換後のJavaScript
    `--src/
        `--index.ts ... 変換前のTypeScript
```

```bash
# TypeScriptを変換してから実行する
npx tsc
node dist/index.js

# TypeScriptを直接実行する
npx tsx src/index.ts
```
