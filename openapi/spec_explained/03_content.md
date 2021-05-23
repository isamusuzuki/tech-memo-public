# Content of Message Bodies

作成日 2021/05/02

[Content of Message Bodies \- OpenAPI Documentation](https://oai.github.io/Documentation/specification-content.html)

## `content` フィールド

このフィールドは、Response Objects, Request Body Objects の 2 つに登場する

このフィールドには、キーに `application/json` のような「メディアタイプ」、バリューに MediaTypeObject が配置されたオブジェクトの配列がアサインされる

```yaml
content:
  application/json: ...
  text/html: ...
```

## Media Type Object

`type` キーには `number`,`string`,`boolean`,`array`,`object` を配置できる

`type: string` のときは、`minLength`と`maxLength`で長さを制限できる。また `enum`で文字列を限定することもできる

```yaml
schema:
  type: string
  enum:
    - Alice
    - Bob
    - Carl
```

`type: number` のときは、`minimum`と`maximum`で範囲を制限できる

```yaml
schema:
  type: integer
  minimum: 1
  maximum: 100
```

`type: object` のときは、`properties` フィールドで、そのオブジェクトのプロパティを定義しないといけない

```yaml
schema:
  type: object
  properties:
    productName:
      type: string
    productPrice:
      type: number
```
