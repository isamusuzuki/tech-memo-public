# Python セロリを学ぶ

作成日 2020/09/14

## 01. Celery とは

Distributed Task Queue

[Celery \- Distributed Task Queue — Celery 4\.4\.2 documentation](https://docs.celeryproject.org/en/stable/)

セロリは、シンプルで、フレキシブル、かつ、信頼性の高い、分散システムです。リアルタイムプロセッシングにフォーカスしたタスクキューですが、タスクスケジュールにも対応しています

[celery \- Qiita](https://qiita.com/kwi/items/bd289aeff0fa5881e797)

> python job queue 処理のフレームワーク。\
> worker daemon を待機させて async 処理の仕組みを作ったり、\
> beat daemon を起動して定時バッチ処理を組んだりする。
>
>- backend は rabbitmq cluster や redis cluster を使う。\
> RDB 使えるけど使わないのが best practice
>- worker daemon 起動。一台に複数起動してもよし。クラスター分散させて起動してもよし。
>
> 処理フロー的には AWS lambda みたいな処理部分に、タスクスケジューラがついたものと考えても近い。

## 02. "Fisrt Steps with Celery"を写経する

[First Steps with Celery — Celery 4\.4\.2 documentation](https://docs.celeryproject.org/en/stable/getting-started/first-steps-with-celery.html#first-steps)

### Choosing a Broker (RabbitMQ)

```bash
docker run -d -p 5672:5672 rabbitmq
# => Starting rabbitmq-server: SUCCESS
```

### Installing Celery

```bash
pip install celery
```

### Application

task.py

```python
from celery import Celery

app = Celery('tasks', broker='pyamqp://guest@localhost//')

@app.task
def add(x, y):
    return x + y
```

### Running the Celery worker server

```bash
celery -A task worker --loglevel=info
```

## 03. 情報収集

[Using Celery With Flask \- miguelgrinberg\.com](https://blog.miguelgrinberg.com/post/using-celery-with-flask)

[FlaskでのCelery　 非同期処理 \- Qiita](https://qiita.com/chi-na/items/c72fcae5af44cb647007)

[Django に Celery タスクキューを導入し、遅い処理を利用者に体感させないようにする](https://tech.torico-corp.com/blog/django-celery-handson/)

[\[Django\] データ更新をメールやチャットサービスで通知する \[Celery\] \- Qiita](https://qiita.com/okoppe8/items/b7243a9e1a786237a51e)

[Pythonで非同期でタスクを実行して、モニタリングする環境をDockerで構築する｜shimakaze\_soft｜note](https://note.com/shimakaze_soft/n/n4a2b63d320ed)
