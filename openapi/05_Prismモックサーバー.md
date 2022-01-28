# Prism モックサーバー

作成日 2021/05/23

## Prism とは

リポジトリ => [https://github.com/stoplightio/prism](https://github.com/stoplightio/prism)

ドキュメント => [https://meta.stoplight.io/docs/prism/README.md](https://meta.stoplight.io/docs/prism/README.md)

## Prism 使い方

```bash
# インストール
npm install @stoplight/prism-cli

npx prism --version
# => 4.2.1

npx prism --help

npx prism mock ./petstore.yaml
```

## Prism の自動データ生成機能

[https://meta.stoplight.io/docs/prism/docs/guides/01-mocking.md#dynamic-response-generation](https://meta.stoplight.io/docs/prism/docs/guides/01-mocking.md#dynamic-response-generation)

`x-faker`キーバリューを挟む

```yaml
Pet:
  type: object
  properties:
    id:
      type: integer
      format: int64
    name:
      type: string
      x-faker: name.firstName
      example: doggie
    photoUrls:
      type: array
      items:
        type: string
        x-faker: image.imageUrl
```

自動データ生成機能を有効にするには、リクエストヘッダーに呪文を入れる

```bash
GET http://localhost:4010/pets HTTP/1.1

# 自動データ生成機能を有効にする
GET http://localhost:4010/pets HTTP/1.1
Prefer: dynamic=true
```
