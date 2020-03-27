# CFunctions を管理する

作成日 2020/03/27

## 01. gcloud コマンドを使う

リファレンス => [https://cloud.google.com/sdk/gcloud/reference/functions/deploy](https://cloud.google.com/sdk/gcloud/reference/functions/deploy)

### デプロイ済みの関数の一覧を表示する

```bash
gcloud functions list
```

### 関数をデプロイする

```bash
gcoud functions deploy NAME\
  --region REGION
  --memory MEMORY
  --timeout TIMEOUT
  --runtime RUNTIME
  TRIGGER
  --env-vars-file FILE_PATH
```

#### NAME

関数名。`--entry-point`で別途指定しない限り、同名の関数名が必要

#### REGION

`asia-northeast1`など。省略したら`us-entral1`

#### MEMORY

`256MB`, `512MB`, `1024MB`など。省略したら`256MB`

#### TIMEOUT

30 秒は`30s`と書く。540 秒以上は設定できない。省略したら 60 秒

#### RUNTIME

`python37`, `nodejs10`のいずれか

#### TRIGGER

`--trigger-http`, `--trigger-topic`, `--trigger-bucket`, `--trigger-event --trigger-resource` のいずれか

#### FILE_PATH

`.env.yaml`ファイルを指定すると、環境変数として取り込まれる
