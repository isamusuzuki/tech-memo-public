# Node.js を使って、Cloud Functions を作成する

作成日 2019/09/11

## 01. ドキュメント「Cloud Functions を書く」を読む

[https://cloud.google.com/functions/docs/writing/?hl=ja#structuring_source_code](https://cloud.google.com/functions/docs/writing/?hl=ja#structuring_source_code)

### モジュールとしての Cloud Functions

許容される構成は以下の通り

-   1 つ以上の関数をエクスポートする index.js ファイル
-   1 つ以上の関数をエクスポートする app.js ファイル、\
    および `"main": "app.js"`を格納した package.json ファイル
-   foo.js ファイルから 1 つ以上の関数をインポートしてから、\
    1 つ以上の関数をエクスポートする index.js ファイル

エクスポートされた関数を`require()`で読み込める限り、\
どのような構成でも機能する

### Cloud Functions のタイプ

```javascript
// HTTP関数
exports.helloHttp = (req, res) => {
    res.send(`Hello ${req.body.name || 'World'}!`);
};

// バックグラウンド関数
exports.helloBackground = (event, callback) => {
    callback(null, `Hello ${event.data.name || 'World'}!`);
};
```

## 02. ドキュメント「Node.js での依存関係の指定」を読む

[https://cloud.google.com/functions/docs/writing/specifying-dependencies-nodejs?hl=ja](https://cloud.google.com/functions/docs/writing/specifying-dependencies-nodejs?hl=ja)

関数の依存関係を指定するには、`package.json`ファイルにその依存関係を追加する

`package.json`ファイル

```json
{
    "dependencies": {
        "uuid": "^3.0.1"
    }
}
```

`index.js`ファイル

```javascript
const uuid = require('uuid');

// Return a newly generated UUID in the HTTP response.
exports.getUuid = (req, res) => {
    res.send(uuid.v4());
};
```

関数がデプロイされると、Cloud Functions は`npm install --production` コマンドを使い、\
`package.json`ファイル内で宣言されている依存関係をインストールする

## 03. ドキュメント「HTTP 関数」を読む

[https://cloud.google.com/functions/docs/writing/http?hl=ja](https://cloud.google.com/functions/docs/writing/http?hl=ja)

### HTTP リクエストの解析

| Content-Type                      | Body               | 取得方法          | 結果       |
| --------------------------------- | ------------------ | ----------------- | ---------- |
| application/json                  | `{"name": "John"}` | request.body.name | John       |
| application/x-www-form-urlencoded | `name=John`        | request.body.name | John       |
| text/plain                        | `my text`          | request.body      | my text    |
| appplication/octet-stream         | `my text`          | request.body      | RAW バイト |

| 取得方法            | 結果                                   |
| ------------------- | -------------------------------------- |
| request.method      | HTTP メソッド (GET, POST, PUT, DELETE) |
| request.get('name') | ヘッダーの値                           |
| request.query.foo   | GET メソッドの値                       |
| request.body.text   | POST データの解析後の値                |
| request.rawBody     | リクエストの RAW バイト数              |

## 04. ドキュメント「Node.js 10 ランタイム」を読む

[https://cloud.google.com/functions/docs/concepts/nodejs-10-runtime?hl=ja](https://cloud.google.com/functions/docs/concepts/nodejs-10-runtime?hl=ja)

### ランタイムの選択

デプロイ時に関数用に Node.js 10 ランタイムを選択できる

```bash
gcloud functions deploy NAME --runtime nodejs10 --trigger-http
```

### 環境変数を使う

-   プロジェクトフォルダ内に`.env.yaml`ファイルを作成し、そこに YAML 形式で変数を書いておく
-   gcloud コマンドでデプロイするとき、このファイルを指定すると、環境変数として登録される

``.env.yaml`ファイル

```yaml
TEST_USERNAME: taro@example.com
TEST_PASSWORD: abcdefg
```

`sandbox/index.js`ファイル

```javascript
exports.coconut1 = async (req, res) => {
    const username = process.env.TEST_USERNAME;
    const password = proceses.env.TEST_PASSWORD;
    console.log(username);
    console.log(password);
    res.json({ username: username, password: password });
};
```

gcloud コマンドでデプロイする

```bash
gcloud functions deploy coconut1\
  --region asia-northeast1\
  --runtime nodejs10\
  --env-vars-file .env.yaml\
  --trigger-http
```

## 05. PubSub をトリガーにする

`sandbox/index.js`ファイル

```javascript
exports.coconut2 = async (event, context) => {
    const json_str = event.data
        ? Buffer.from(event.data, 'base64').toString()
        : '{"kanri_no": "no-data"}';
    console.log(json_str);
    json_obj = JSON.parse(json_str);
    console.log(json_obj['kanri_no']);
};
```

gcloud コマンドでデプロイする

```bash
# あらかじめPubSubトピックを作成する
gcloud pubsub topics create sandbox-pubsub

# ファンクションをデプロイする
gcloud functions deploy coconut2\
  --region asia-northeast1\
  --runtime nodejs10\
  --trigger-resource sandbox-pubsub\
  --trigger-event google.pubsub.topic.publish

# PubSubトピックにメッセージを送信する
gcloud pubsub topics publish sandbox-pubsub\
  --message '{"kanri_no": "12345abc"}'
```
