# Structure of an OpenAPI Document

作成日 2021/05/02

[Structure of an OpenAPI Document \- OpenAPI Documentation](https://oai.github.io/Documentation/specification-structure.html)

## Document Syntax

OpenAPI ドキュメントとは、`openapi.json` または `openapi.yaml` と呼ばれるテキストファイルのことである。

YAML では、以下の属性タイプに加えてコメントを扱える

- Number
- String
- Boolean
- `null` value
- Array
- Object

```yaml
anObject:
  aNumber: 42
  aString: This is a string
  aBoolean: true
  nothing: null
  arrayOfNumbers:
    - 1
    - 2
    - 3
```

## Minimal Document Structure

```yaml
openapi: 3.1.0
info:
  title: A minimal OpenAPI document
  version: 0.0.1
paths: {}
```

- ルートオブジェクトの中で必須項目は `openapi`, `info`, `paths`の 3 つ
- info オブジェクトの中の必須項目は `title`, `version` の 2 つ
