# otz

作成日 2025/11/18

## 1. 解説記事を読む

[OpenAPI から Zod スキーマを生成する otz の紹介 #TypeScript - Qiita](https://qiita.com/seiya8bit/items/bd4041e09a6508c5870e)

> OpenAPI の仕様(以下 OAS)を基に Zod スキーマを生成する otz というツールを作りました。
>
> 私は普段 OAS を書くために TypeSpec を使用しているため、 OAS を楽に書きつつ Zod スキーマも自動生成されるようなアプローチを好みます。

## 2. 公式サイト（英語）を読む

[@seiya8bit/otz - npm](https://www.npmjs.com/package/@seiya8bit/otz)

> otz is a command-line tool that generates Zod schemas from your OpenAPI definitions. Maps OpenAPI types and formats to Zod schemas, and generates validation messages useful for UI forms.

## 3. サンプルコードのありか

[otz/examples/petstore at main · seiya8bit/otz](https://github.com/seiya8bit/otz/tree/main/examples/petstore)

```text
--petstore/
    |--gen/
    |   `--openapi.ts   ... otzから生成されたZodのスキーマ
    |--output/
    |   `--openapi.yaml ... TypeSpecから生成されたOpenAPIのコード
    |--main.tsp         ... TypeSpec定義のエントリーポイント　
    `--tspconfig.yaml   ... TypeSpecコンパイラーの設定
```
