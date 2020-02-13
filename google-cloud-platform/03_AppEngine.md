# App Engine

作成日 2020/01/08

## 01. スタンダード環境とフレキシブル環境

ドキュメント => [App Engine 環境の選択](https://cloud.google.com/appengine/docs/the-appengine-environments)

解説記事 => [GCE と GAE、どっち使えばいいの？という人のための GCP 運用診断 \- Qiita](https://qiita.com/defunty/items/f837c9e09c8ccc6033b3)

## 02. Flask アプリをデプロイしてみる

### ファイルを用意する

```text
-- avocado
    |--app.yaml
    |--main.py
    `--requirements.txt
```

`app.yaml`ファイル

```yaml
runtime: python37
env: standard
service: default
entrypoint: gunicorn -b :$PORT main:app

automatic_scaling:
    min_idle_instances: automatic
    max_idle_instances: automatic
    min_pending_latency: automatic
    max_pending_latency: automatic
```

`main.py`ファイル

```python
from flask import Flask, jsonify

app = Flask(__name__)
app.config['JSON_AS_ASCII'] = False

@app.route('/')
def index():
    return jsonify({
        "message": "ハロー、ワールド！"
    })

if __name__ = '__main__':
    app.run()
```

`requirements.txt`ファイル

```text
flask
gunicorn
```

### デプロイする

一番最初にデプロイするアプリのサービスは default でないといけない

```bash
cd ~/avocado

gcloud app deploy
# => Services to deploy:
# =>
# => descriptor:      [C:\Users\<name>\avocado\app.yaml]
# => source:          [C:\Users\<name>\avocado]
# => target project:  [my-test-project-221606]
# => target service:  [default]
# => target version:  [20190520t221607]
# => target url:      [https://my-test-project-221606.appspot.com]
# =>
# =>
# => Do you want to continue (Y/n)? y

gcloud app browse
# => ブラウザが立ち上がりhttps://my-test-project-221606.appspot.comを表示する

gcloud app logs tail
# => サーバーサイドのログを吐き出し続ける
```
