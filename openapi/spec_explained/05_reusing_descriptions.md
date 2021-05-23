# Reusing Descriptions

作成日 2021/05/03

[Reusing Descriptions \- OpenAPI Documentation](https://oai.github.io/Documentation/specification-components.html)

## The Component Object

Component Object は、ルートに配置された `components` で定義され、他の場所で再利用される

`components` キーの下に、`schemas` キーと `parameters` キーがある

`shemas` キーで定義されたものは、Schema Object として再利用できる

`parameters` キーで定義されたものは、Parameter Object として再利用できる

## The Reference Object

Component Object でサポートされているものは、Reference Object に置き換えることができる

$refs キーに、URI がバリューとして組み合わされる。絶対パス、相対パスでもオーケー

```yaml
$ref: 'https://gigantic-server.com/schemas/Monster/schema.yaml'
---
$ref: './another_file.yaml#rowParam'
```

サンプル

```yaml
components:
  schemas:
    coordinate:
      type: integer
      minimum: 1
      maximum: 3
  parameters:
    rowParam:
      name: row
      in: path
      required: true
      schema:
        $ref: '#/components/schemas/coordinate'
    columnParam:
      name: column
      in: path
      required: true
      schema:
        $ref: '#/components/schemas/coordinate'
paths:
  /board/{row}/{column}:
    parameters:
      - $ref: '#/components/parameters/rowParam'
      - $ref: '#/components/parameters/columnParam'
```
