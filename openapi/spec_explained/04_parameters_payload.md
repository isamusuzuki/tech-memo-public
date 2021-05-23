# Parameters and Payload of an Operation

作成日 2021/05/03

[Parameters and Payload of an Operation \- OpenAPI Documentation](https://oai.github.io/Documentation/specification-parameters.html)

- Parameter Object は、Path Item Object の parameters キーに配置される
- RequestBody Object は、Operation Object の requestBody キーに配置される

## The Parameter Object

キー

- `in` ... 必須、{path, query, header}
- `name` ... 必須
- `description` ... オプション
- `required` ... オプション
- `schema` ... オプション

```yaml
paths:
  /user/{id}:
    get:
      parameters:
        - name: id
          in: path
          required: true
---
parameters:
  - name: id
    in: query
    schema:
      type: integer
      minimum: 1
      maximum: 100
```

## The Request Body Object

```yaml
paths:
  /board:
    put:
      requestBody:
        content:
          application/json:
            schema:
              type: integer
              minimum: 1
              maximum: 100
```
