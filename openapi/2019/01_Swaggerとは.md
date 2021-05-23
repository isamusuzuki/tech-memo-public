# Swagger とは

作成日 2018/07/29、更新日 2019/04/12

## Swagger

RESTful API を構築するためのフレームワーク。ドキュメント、サーバーコード、クライアントコード、エディタを提供する

RESTful API インターフェイスを記述するドキュメントの仕様は、OpenAPI イニシアチブが策定している

公式サイト 1 => [https://swagger.io/](https://swagger.io/)

公式サイト 2 => [https://www.openapis.org/](https://www.openapis.org/)

OpenAPISpec 最新版 => [OpenAPI Specification 3.0](https://github.com/OAI/OpenAPI-Specification/blob/OpenAPI.next/versions/3.0.0.md)

## 紹介記事 1

[Swagger とは](https://qiita.com/pi-su/items/4d9f142475a5be893beb)

- SwaggerSpec ... 設計書の仕様、YAML で書く
- SwaggerEditor ... 設計書を記載するためのエディタ
- SwaggerUI ... SwaggerSpec で記載された設計からドキュメントを HTML 形式で自動生成するツール
- SwaggerCodegen ... SwaggerSpec で記載された設計から API のスタブを自動生成
- SwaggerHub ... クラウドサービス

## 紹介記事 2

[Swagger を使ってモックサーバを立てるまでの手順](https://qiita.com/Fendo181/items/c76b618c80836d61afed)

この様に手順としては`Swagger Editer`で`Swagger Specification`を記述して、`SwaggerUI`を見ながら、API の仕様を設計していく方針になると思います。

`SwaggerHub`からサーバファイルを出力してモックサーバを構成してみましょう。`SwaggerHub`の server から、指定の環境を選択してファイルを出力します。

## 紹介記事 3

[OpenAPI \(Swagger\) 超入門 \- Qiita](https://qiita.com/teinen_qiita/items/e440ca7b1b52ec918f1b)

Swagger の基本構造 => YAML 形式で記述していく

- `openapi:` ... セマンティックなバージョニングを記述する
- `info:` ... API のメタデータを記述する
- `servers:` ... API を提供するサーバーを記述する。配列で複数を記述可能
- `paths:` ... API で利用可能なエンドポイントやメソッドを記述する
- `components:` ... API で使用するオブジェクトスキーマを記述する
- `security:` ... API 全体を通して使用可能なセキュリティ仕様を記述する
- `tags:` ... API で使用されるタグのリスト
- `externalDocs:` ... 外部ドキュメントを記述する

`openapi:`, `info:`, `paths:`, `components:`の 4 項目は必須

`openapi:`の例

```yaml
openapi: 3.0.0
```

`info:`の例

```yaml
info:
  title: Sample API
  description: A short description of API.
  termsOfService: http://example.com/terms/
  contact:
    name: API support
    url: http://www.example.com/support
    email: support@example.com
  license:
    name: Apache 2.0
    url: http://www.apache.org/licenses/LICENSE-2.0.html
  version: 1.0.0
```

`servers:`の例

```yaml
servers:
  - url: https://dev.sample-server.com/v1
    description: Development server
  - url: https://stg.sample-server.com/v1
    description: Staging server
  - url: https://api.sample-server.com/v1
    description: Production server
```

`paths:`の例

```yaml
paths:
  /users:
    get:
      tags:
        - users
      summary: Get all users.
      description: Returns an array of User model
      parameters: []
      responses:
        '200':
          description: A JSON array of User model
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
                example:
                  - id: 1
                    name: Johe Doe
                  - id: 2
                    name: Jane Doe
    post:
      tags:
        - users
      summary: Create a new user.
      description: Create a new User
      parameters: []
      requestBody:
        description: user to create
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
            example:
              id: 3
              name: Richard Roe
      responses:
        '201':
          description: CREATED
  /users/{userId}:
    get:
      tags:
        - users
      summary: Get user by ID.
      description: Returns a single User model
      parameters:
        - name: userId
          in: path
          description: user id
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: A single User model
          content:
            application/json:
              schema:
                type: object
                items:
                  $ref: '#/components/schemas/User'
                example:
                  id: 1
                  name: John Doe
```

`components:`の例

```yaml
components:
  schemas:
    User:
      type: object
      required:
        - id
      properties:
        id:
          type: integer
          format: int64
        name:
          type: string
    Product:
      type: object
      required:
        - id
        - price
      properties:
        id:
          type: integer
          format: int64
          example: 1
        name:
          type: string
          example: Laptop
        price:
          type: integer
          example: 1200
```

`security:`の例

```yaml
security:
  - api_key: []
  - users_auth:
      - write:users
      - read:users
```

`tags:`の例

```yaml
tags:
  - name: users
    description: Access to Users
  - name: products
    description: Access to Products
```

`externalDocs:`の例

```yaml
externalDocs:
  description: Find more info here
  url: https://example.com
```
