# Python スクリプトから Cloud Storage を利用する

作成日 2020/03/25

## 01. ライブラリを組み込む

[Cloud Storage Client Libraries  \|  Google Cloud](https://cloud.google.com/storage/docs/reference/libraries)

### pip を使ってライブラリをインストールする

```bash
pip install google-cloud-storage
```

### 認証情報をセットする

1. ストレージと同じプロジェクトにサービスアカウントを作成する
1. 「ストレージのオブジェクト作成者」「ストレージ オブジェクト閲覧者」というロールを追加する
1. 認証キー（JSON ファイル）を作成して、ダウンロードする
1. 環境変数に、認証キーのありかを追加する `GOOGLE_APPLICATION_CREDENTIALS=[PATH]`
1. コードを書く

```python
from google.cloud import storage

client = storage.Client()
bucket_name = 'my-new-bucket'
bucket = client.create_bucket(bucket_name)
```

## 02. オブジェクトのアップロード

ドキュメント => [オブジェクトのアップロード  \|  Cloud Storage  \|  Google Cloud](https://cloud.google.com/storage/docs/uploading-objects#storage-upload-object-code-sample)

`blob.upload_from_filename()`を使う

```python
from google.cloud import storage

client = storage.Client()
bucket = client.bucket(bucket_name)

# destination_blob_nameは、クラウド上のオブジェクト名
# オブジェクト名にはスラッシュ(/)が使え、仮想のディレクトリになる
# あくまで仮想なので、冒頭のスラッシュは不要
blob = bucket.blob(destination_blob_name)

# source_file_nameは、ローカルにあるファイルのパス名
blob.upload_from_filename(source_file_name)
```

## 03. オブジェクトのダウンロード

ドキュメント => [オブジェクトのダウンロード  \|  Cloud Storage  \|  Google Cloud](https://cloud.google.com/storage/docs/downloading-objects)

`blob.download_to_filename()`を使う

```python
from google.cloud import storage

storage_client = sotrage.Client()
bucket = storage_client.bucket(bucket_name)

# source_file_nameは、クラウド上のオブジェクト名
blob = bucket.blob(source_blob_name)

# destination_blob_nameは、ローカルに作成するファイルのパス名
blob.download_to_filename(destination_file_name)
```

## 04. Blob クラスを調べる

[Blobs / Objects — google\-cloud\-storage documentation](https://googleapis.dev/python/storage/latest/blobs.html)

- `blob.upload_from_file()` ... オブジェクトからアップロードする
- `blob.upload_from_filename()` ... ファイル名からアップロードする
- `blob.upload_from_string()` ... 提供された文字列をコンテンツとしてアップロードする
- `blob.download_to_file()` ... オブジェクトにダウンロードする
- `blob.download_to_filename()` ... ローカルファイルとしてダウンロードする
- `blob.download_as_string()` ... コンテンツをダウンロードして文字列として使う

ダウンロードしたコンテンツを、そのまま Web サーバーのレスポンスとして返したい\
そのような場合は、`blob.download_to_file()`を使うはず

```python
from google.cloud.storage import Blob

client = storage.Client(project="my-project")
bucket = client.get_bucket("my-bucket")
encryption_key = "c7f32af42e45e85b9848a6a14dd2a8f6"
blob = Blob("secure-data", bucket, encryption_key=encryption_key)
blob.upload_from_string("my secret message.")
with open("/tmp/my-secure-file", "wb") as file_obj:
    blob.download_to_file(file_obj)
```
