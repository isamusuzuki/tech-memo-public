# Cloud Storage を Python スクリプトから利用する

作成日 2020/03/25

## 01. ライブラリを組み込む

[Cloud Storage Client Libraries  \|  Google Cloud](https://cloud.google.com/storage/docs/reference/libraries)

### pipを使ってライブラリをインストールする

```bash
pip install google-cloud-storage
```

### 認証情報をセットする

1. ストレージと同じプロジェクトにサービスアカウントを作成する
1. 「ストレージのオブジェクト作成者」「ストレージ オブジェクト閲覧者」というロールを追加する
1. 認証キー（JSONファイル）をダウンロードする
1. 環境変数に、認証キーのありかを追加する `GOOGLE_APPLICATION_CREDENTIALS=[PATH]`
1. コードを書く

```python
from google.cloud import storage

client = storage.Client()
bucket_name = 'my-new-bucket'
bucket = client.create_bucket(bucket_name)
```

## 02. ファイルをアップロードする

```python
from google.cloud import storage

client = storage.Client()
bucket_name = 'my-bucket'
bucket = client.bucket(bucket_name)

blob = bucket.blob(destination_blob_name)
blob.upload_from_filename(source_file_name)
```

ドキュメント => [Blobs / Objects — google\-cloud\-storage documentation](https://googleapis.dev/python/storage/latest/blobs.html)
