# 署名付き URL を作成する

作成日 2020/04/24

## 公式ドキュメントを読む

[Blobs / Objects — google\-cloud\-storage documentation](https://googleapis.dev/python/storage/latest/blobs.html)

`blob.genarate_signed_url()`が使えることがわかった

引数の解説

- `expiration` ... datetime.datetime, datetime.timedelta が使える
- `credentials` ... google.auth.creadentials.Credentials が使える
- `response_disposition` ... ファイル名を指定してダウンロードさせたいときに使う

## Python サンプルコード

```python
from datetime import timedelta

import google.auth
from google.cloud import storage

# Cloud Storageの準備
client = storage.Client()
bucket = client.bucket(BUCKET_ZIP)

# ファイルの存在確認
blob = bucket.blob(zip_name)
if not blob.exists():
    return None
else:
    an_hour_from_now = timedelta(hours=1)
    credentials, project_id = google.auth.default()
    rd_text = f'attachment; filename={zip_name}'
    url = blob.generate_signed_url(
        expiration=an_hour_from_now, 
        credentials=credentials,
        response_disposition=rd_text
    )
    return url
```

## どハマりしたこと

ローカルではうまくいっていたコードが、Cloud Functions にデプロイしたとたんにエラーを吐くようになった

```text
AttributeError: you need a private key to sign credentials.

the credentials you are currently using <class 'google.auth.compute_engine.credentials.Credentials'> just contains a token.

see https://googleapis.dev/python/google-api-core/latest/auth.html#setting-up-a-service-account for more details.
```

Cloud Functions が利用しているサービスアカウントで、JSON キーを作成し、ダウンロードされた JSON ファイルを一緒にデプロイすることで解決した

もちろん `.env.yaml` に以下のように書いて、Cloud Functions にデプロイするコマンドに `--env-vars-file FILE_PATH` 引数を追加したことは言うまでもない。ちょっと心配した `FILE_PATH` はファイル名だけでうまくいった

```yaml
GOOGLE_APPLICATION_CREDENTIALS: DOWNLOADED-JSON-KEY-FILE-NAME.json
```
