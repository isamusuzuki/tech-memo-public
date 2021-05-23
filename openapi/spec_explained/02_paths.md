# API Endpoints

作成日 2021/05/02

[API Endpoints \- OpenAPI Documentation](https://oai.github.io/Documentation/specification-paths.html)

## The Endpoints list

API エンドポイントは、OAS では `paths` と呼ばれる

Paths Object > Path Item Object > Operation Object > Responses Object > Response Object

- path は `/` から始まらないといけない
- path item は `get`,`put`,`delete` などがアサインされる
- operation には `summary`,`description`,`responses` などが含まれる
- responses は response を含む
- response は `2xx`,`3xx`,`4xx`, `5xx` といった、HTTP レスポンスコードでないといけない
- response には `description`, `content` がアサインされる

```yaml
paths:
  /board:
    get:
      summary: Summary
      description: Description
      operationId: get-board
      responses:
        '200':
          description: 'OK'
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/status'
```
