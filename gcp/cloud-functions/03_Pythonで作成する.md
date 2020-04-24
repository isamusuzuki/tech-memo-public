# Python を使って、Cloud Functions を作成する

作成日 2019/11/22、更新日 2019/12/06

## 01. Python ランタイムのフォルダ構造

ランタイムごとにフォルダ構造が決まっている

### main.py ファイルはマスト

関数のエントリーポイントは、プロジェクトのルートにある\
`main.py`の中に定義されている必要がある

```bash
gcoud functions deploy NAME --runtime RUNTIME TRIGGER [FLAGS...]
```

`gcloud functions deploy`コマンドの最初の引数である NAME は、\
`main.py`で定義されている関数名である

#### デプロイするときの注意事項

必ず`main.py`があるフォルダまで行ってから、デプロイする

```bash
cd sandbox1

# 現在の設定を確認することも忘れずに
gcloud config configurations list

gcloud functions deploy sandbox1\
  --region asia-northeast1\
  --runtime python37\
  --trigger-http
```

### 以下のソースコードはすべて有効

**パターン 1**\
プロジェクトのルートに`main.py`が 1 つだけあって、\
1 つ以上の関数を定義している

```text
--release1/
    `--main.py
```

**パターン 2**\
`main.py`の他に、`requirements.txt`があって、\
パッケージの依存を指定している

```text
--release1/
    |--main.py
    `--requirements.txt
```

**パターン 3**\
`main.py`がローカル依存のコードをインポートしている

```text
--release1/
    |--main.py
    `--mylocalpackage/
        |--__init__.py
        `--myscript.py
```

## 02. Ajax サーバーを設置する

- HTTP トリガーの関数の引数は、Flask の request モジュールである
- HTTP トリガーの関数の戻値は、Flask の response モジュールである

`sandbox1/main.py`ファイル

```python
import json

from flask import jsonify


def sandbox1(request):
    response = jsonify({'meassage': 'このメソッドは受け付けられません'})

    if request.method == 'GET':
        response = jsonify({'message': 'GETメソッドで来ましたね'})
    elif request.method == 'POST':
        message = 'POSTメソッドで来ましたね'
        data = request.data.decode('utf-8')
        data_dict = json.loads(data)
        if 'name' in data_dict:
            message += f'。{data_dict["name"]}さん'
        response = jsonify({'message':  message})
    elif request.method == 'PUT':
        message = 'PUTメソッドで来ましたね'
        data = request.data.decode('utf-8')
        data_dict = json.loads(data)
        if 'name' in data_dict:
            message += f'。{data_dict["name"]}さん'
        response = jsonify({'message':  message})
    elif request.method == 'DELETE':
        response = jsonify({'message': 'DELETEメソッドで来ましたね'})

    response.headers.set('Access-Control-Allow-Origin', '*')
    response.headers.set('Access-Control-Allow-Headers',
                         'Origin, X-Requested-With, Content-Type, Accept')
    response.headers.set('Access-Control-Allow-Methods',
                         'GET, POST, PUT, DELETE')
    return response
```

## 03. 環境変数を使う

- Cloud Functions では、プロジェクトフォルダ内に`.env.yaml`ファイルを作成して、そこに変数を書いておく
- gcloud コマンドでデプロイするとき、このファイルを指定すると、環境変数として登録される

`.env.yaml`ファイル

```yaml
DB_HOST_NAME: aaaa
DB_USER_NAME: bbbb
DB_PASSWORD: cccc
DB_DB_NAME: dddd
```

gcloud コマンドでデプロイするときに、.env.yaml ファイルを指定する

```bash
gcloud functions deploy sandbox2\
  --region asia-northeast1\
  --runtime python37\
  --env-vars-file .env.yaml\
  --trigger-http
```

`sandbox/main.py`ファイル

```python
import os

import pymysql


def sandbox2(request):
    DB_HOST_NAME = os.environ.get('DB_HOST_NAME')
    DB_USER_NAME = os.environ.get('DB_USER_NAME')
    DB_PASSWORD = os.environ.get('DB_PASSWORD')
    DB_DB_NAME = os.environ.get('DB_DB_NAME')

    conn = pymysql.connect(
        host=DB_HOST_NAME,
        user=DB_USER_NAME,
        password=DB_PASSWORD,
        db=DB_DB_NAME,
        charset='utf8m64',
        cursorclass=pymysql.coursors.DictCursor
    )

    try:
      with conn.cursor() as cursor:
        sql = 'sql-query'
        cursor.execute(sql, ('a' 'b', 'c'))
        conn.commit()
        print('done')
    finally:
      conn.close()
```

## 04. gcloud コマンドで初めてデプロイするときの注意事項

以下のようなメッセージが出た

```bash
gcloud functions deploy kerai_uketsuke_v1 \
--region asia-northeast1 \
--runtime python37 \
--trigger-http
# => Allow unauthenticated invocations of
#    new function [kerai_uketsuke_v1]?
```

invocation とは「呼び出し」のこと。HTTP トリガーは、認証なしに誰が叩いてもいいのかということを聞いている

## 05. gcloud コマンドはフォルダにあるファイルをすべてデプロイする

長くなった SQL クエリーをファイルとして外出しして、\
Python スクリプトでそのファイルの中身を読み込んでから、\
データベースに投げるようにしてみた

gcloud コマンドを使ってデプロイしたときに、\
拡張子が`.sql`のファイルは、ちゃんとアップロードされるのかが不安であったが\
結論としては、なんの問題もなかった。gcloud コマンドを実行したフォルダにある\
ファイルとサブフォルダは、すべてアップロードしていることがわかった
