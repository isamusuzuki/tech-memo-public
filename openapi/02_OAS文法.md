# OAS 文法を学ぶ

作成日 2021/04/25

## 01. 公式ドキュメントを読む

[The OpenAPI Specification Explained \- OpenAPI Documentation](https://oai.github.io/Documentation/specification.html)

- ドキュメント形式 => JSON, YAML
- ドキュメント情報 => `openapi`, `info`
- API エンドポイント => `paths`, `responses`
- メッセージの中身 => `content`, `schema`
- 引数とペイロード => `parameters`, `requestBody`
- 再利用可能な定義 => `components`, `$ref`
- 使用例 => `example`, `exmaples`
- API サーバー => `servers`

## 02. サンプルコードから読み解く大事なポイント

※ 02a_petstore.yml を参照のこと

### API エンドポイントの定義

- `paths` ＞ スラッシュ始まりのパス ＞ GET,POST,PUT,DELETE メソッド ＞ `responses`

```yaml
paths:
  /pets:
    get:
      response:
```

### 応答メッセージの定義

- `responses` ＞ HTTPステータスコード ＞ `content` ＞ MIMEタイプ ＞ `schema`

```yaml
responses:
  '200':
    content:
      application/json:
        schema:
```

### 再利用可能な定義へのリンク

- `$ref` ＞ `components/schemas`

```yaml
schema:
  $ref: '#/components/schemas/Pets'
components:
  schemas:
    Pets:
```

### オブジェクトの定義

```yaml
type: object
properties:
```

### 配列の定義

```yaml
type: array
items:
```
