# Python コードで S3 バケットにファイルをアップする

作成日 2019/12/11

## 01. アクセスキー ID とシークレットアクセスキーの入手

これは、一番最初にやっておくべきこと\
AWS マネジメントコンソールにログインする

### 先にポリシーを作成する

AWS マネジメントコンソール ＞ IAM を検索 ＞ IAM ページ\
左枠 ＞ ポリシー ＞ 「ポリシーの作成」ボタン

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::target-bucket-name/*"
        }
    ]
}
```

### 次にユーザーを作成する

左枠 ＞ ユーザー ＞ 「ユーザーを追加」ボタン\
ユーザー名を入力する\
アクセスの種類は、「プログラムによるアクセス」だけにチェックを入れる\
「次のステップ：アクセス権限」ボタン\
「既存のポリシーを直接アタッチ」タブ ＞ ポリシーのフィルタで「ユーザーによる管理」にチェック入れる\
あらかじめ作成しておいたポリシーをユーザーにアタッチする\
「次のステップ：タグ」ボタン\
「次のステップ：確認」ボタン\
「ユーザーの作成」ボタン\
アクセスキーとシークレットアクセスキーをメモする

## 02. boto3 のインストール

```bash
pip install boto3
```

boto3 をインスタンス化するときに必須の引数 3 つは、すべて環境変数に設定する。\
その場合は、引数そのものを省略できる

```bash
export AWS_ACCESS_KEY_ID=ABCDEFG123456
export AWS_SECRET_ACCESS_KEY=ABCDEFG123456
export AWS_DEFAULT_REGION=ap-northeast-1
```

## 03. boto3 の使い方

ドキュメント => [S3 — Boto 3 Docs 1\.10\.29 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html)

-   S3.Client.upload_file(Filename, Bucket, Key)
-   S3.Bucket.upload_file(Filename, Key)

```python
import boto3

s3 = boto3.client('s3')
s3.upload_file(local_path, bucket_name, remote_path)
```

## 04. Python のサンプルコード

```python
import datetime

import boto3

import pytz

# テキストファイルの中身を作成
jpn_text = (
    '文字コードをBOM付きのUTF-8にしておけば、\n'
    'Windowsマシンでダウンロードしても、\n'
    '文字化けせずに読めるということをテストしています。\n'
    'このファイルは、削除していただいて構いません。'
)

# ファイル名を作成
jpn_now = datetime.datetime.now(pytz.timezone('Asia/Tokyo'))
file_name = f'{jpn_now.strftime("%Y%m%d-%H%M%S")}_test.txt'
local_file_path = f'./temp/{file_name}'
remote_file_path = f'test/{file_name}'

# ローカルにテキストファイルを作成
with open(local_file_path, mode='w', encoding='utf_8_sig') as f:
    f.write(jpn_text)
    print('サンプルのテキストファイルを生成しました')

# ローカルからAWS S3にアップロードする
s3 = boto3.resource('s3')
bucket = s3.Bucket('target-bucket-name')
bucket.upload_file(local_file_path, remote_file_path)
print('テキストファイルがアップロードされました')
```
