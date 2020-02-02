# Cloud Pub/Sub をトリガーにする

作成日 2019/12/17

[Google Cloud Pub/Sub トリガー  \|  Cloud Functions のドキュメント](https://cloud.google.com/functions/docs/calling/pubsub)

> Cloud Functions can be triggered by messages published to Cloud Pub/Sub topics in the same GCP project as the function.

## 01. Python37 のサンプルコード

```python
import base64

def hello_pubsub(data, context):
    """Background Cloud Function to be triggered by Pub/Sub.
    Args:
         data (dict): The dictionary with data specific to this type of event.
         context (google.cloud.functions.Context): The Cloud Functions event
         metadata.
    """
    if 'data' in data:
        name = base64.b64decode(data['data']).decode('utf-8')
    else:
        name = 'World'
    print(f'Hello {name}!')
```

-   pubsub メッセージは、base64 でエンコードされた状態で、data 引数の data キーの値として、\
    送られてくる
-   pubsub メッセージは、必ずしも JSON データではない。ただの文字列である場合もある
-   とはいえ、pubsub メッセージは、JSON データのほうがやっぱり便利

## 02. Cloud Function をデプロイする

```bash
# デプロイする前に、現在アクティブになっている設定を確認する
gcloud config configurations list

# 設定を変更する
gcloud config configurations activate SETTINGS

# 現在アクティブになっているプロジェクトの関数群を確認する
gcloud functions list

# デプロイする
gcloud functions deploy hello_pubsub \
--region asia-northeast1 \
--memory 1024MB \
--timeout 90s \
--runtime python37 \
--trigger-topic hello_pubsub_topic \
--env-vars-file .env.yaml
```

## 03. PubSub にメッセージを投げて、Cloud Functions を実行させる

```bash
# トピックを一覧表示する
gcloud pubsub topics list

# トピックにメッセージを投げる
gcloud pubsub topics publish hello_pubsub_topic \
--message "Taro Okamoto"

gcloud pubsub topics publish hello_pubsub_topic \
--message "{\"name\": \"Taro Okamoto\"}"
```

## 04. Cloud Scheduler にジョブを登録して、Cloud Functions を定期実行させる

```bash
# 登録済みのジョブの一覧を表示する
gcloud scheduler jobs list

# 登録する
gcloud scheduler jobs create pubsub hello_pubsub \
--schedule="2 0 * * *" \
--topic=hello_pubsub_topic \
--message-body="Taro Okamoto" \
--time-zone="Asia/Tokyo"

# 更新する
gcloud scheduler jobs update pubsub hello_pubsub \
--schedule="2 0 * * *" \
--topic=hello_pubsub_topic \
--message-body="{\"name\": \"Taro Okamoto\"}" \
--time-zone="Asia/Tokyo"
```

Cloud Scheduler で、PubSub にメッセージを投げられることがわかったので、今後は PubSub トリガーをメインにする

Cloud Scheduler は、`gcloud scheduler jobs create`コマンドを繰り返すことが出来ない。エラーになる。
2 回目以降は、`gcloud scheduler jobs update`コマンドを使う

gcloud scheduler jobs コマンドのリファレンス => [gcloud scheduler jobs  \|  Cloud SDK  \|  Google Cloud](https://cloud.google.com/sdk/gcloud/reference/scheduler/jobs/)

## 05. Cloud Functions をローカルでテストする

Cloud Pub/Sub をトリガーにした Cloud Functions は、ローカルでのテストもやりやすい

`project/main.py`ファイル

```python
from apps.util import get_params
from apps.real import real_function


def main_function(data, context):
    params = get_params(data)
    env = params['env'] if 'env' in params else 'cloud'
    debug = params['debug'] if 'debug' in params else False
    result = real_function(env, debug)
    print(result)
```

main_function は、実体というよりも、Cloud Functions に登録する名義であり、\
2 つの引数 data, context を処理して、本物の関数に渡している

`project/local.py`ファイル

```python
import base64
import json

from main import main_function

def test(params):
    dumped_params = json.dumps(params)
    data = {
        'data': base64.b64encode(
            dumped_params.encode('utf-8')
        )
    }
    context = {}
    registered_function(data, context)

if __name__ == '__main__':
    params = {
        'env': 'local',
        'debug': True
    }
    test(params)
```

Cloud Functions に渡すメッセージも、自分で base64 エンコードすれば\
pubsub 経由で渡したかのようにフェイクできる

env や debug といったパラメーターでデータベースへの接続設定や、\
データを書き込むテーブルを切り替えられるように作りこんでおけば、\
似たようなコードをテスト用にわざわざつくる必要はない
