# Python で Cloud Firestore を使う

作成日 2019/11/11

## 01. 準備作業

### ローカルから使うにはサービスアカウントキーが必要

- 新しくサービスアカウントを作成する
- サービスアカウントの権限に「Cloud Datastore ユーザー」を追加する
- キーの作成を実行する。キータイプは JSON を選ぶ
- JSON ファイルをダウンロードして、ホームフォルダに置く
- `.bashrc` ファイルに以下を書き込む（Git Bash を利用している場合）

```bash
export GOOGLE_APPLICATION_CREDENTIALS=c:\Users\<user-name>\<service-account-key>.json
```

### Python ライブラリのインストール

```bash
pip install google-cloud-firestore
```

## 02. Python ライブラリのドキュメント

[https://googleapis.dev/python/firestore/latest/index.html](https://googleapis.dev/python/firestore/latest/index.html)

### CollectionReference クラス

[https://googleapis.dev/python/firestore/latest/collection.html](https://googleapis.dev/python/firestore/latest/collection.html)

> class google.cloud.firestore_v1.colection.CollectionReference
>
> where(field_path, op_string, value)\
> Create a "where" query with this collection as parent.\
> Return type: Query

検索結果は Query クラスになっている

### Query クラス

[https://googleapis.dev/python/firestore/latest/query.html](https://googleapis.dev/python/firestore/latest/query.html)

> class google.cloud.firestore_v1.query.Qiery
>
> select(field_paths)\
> Project documents mathing query to limited set of fields.
>
> stream()\
> Read the documents in the collection that match this query.

stream()メソッドで得られるものは、ジェネレータ\
ジェネレータを for ループで回して得られるものは、DocumentSnapshot クラス

### DocumentSnapshot クラス

[https://googleapis.dev/python/firestore/latest/document.html](https://googleapis.dev/python/firestore/latest/document.html)

> class google.cloud.firestore_v1.document.DocumentSnapshot
>
> get(field_path)\
> Get a value from the snapshot data.
>
> to_dict()\
> Retrieve the data contained in this snapshot.

### DocumentReference クラス

[https://googleapis.dev/python/firestore/latest/document.html](https://googleapis.dev/python/firestore/latest/document.html)

> class google.cloud.firestore_v1.document.DocumentReference
>
> A reference to a document in a Firestore database.
>
> The document may already exist or can be created by this class.

- `create(document_data)` ... Create the current document in the Firestore database.
- `delete()` ... Delete the current document in the Firestore database.
- `get()` ... Retrieve a snapshot of the current document.
- `set(document_data)` ... Replace the current document in the Firestore database.
- `update(field_updates)` ... Update an existing document in the Firestore database.

理解したこと

- ドキュメントがあるところで、`create()`メソッドを使うと、エラーが起こる
- まだドキュメントがないところで、`update()`メソッドを使うと、エラーが起こる
- ドキュメントがなくても、`set()`メソッドは、エラーを起こさない
- `set()`メソッドは、全データを入れ替える。`update()`メソッドは、重なってないところは置き換えない

### Helpers クラス

[https://googleapis.dev/python/google-api-core/latest/helpers.html](https://googleapis.dev/python/google-api-core/latest/helpers.html)

> class google.api_core.datetime_helpers.DatetimeWithNanoSeconds\
> Bases: datetime.datetime

Firestore に格納されている時間データは、DatetimeWithNanoSeconds クラスである

## 03. Python サンプルコード

### `set()`メソッドを使って、指定したドキュメントにデータを登録する

```python
import datetime

from google.cloud import firestore

import pytz

jpn_now = datetime.datetime.now(pytz.timezone('Asia/Tokyo'))
year_number, week_number, youbi_number = jpn_now.isocalendar()
current_name = f'Y{year_number}-W{week_number}-D{youbi_number}'

# サンプルコードは、すべてここから始まる
db = firestore.Client(project='test-project')
doc_ref = db.collection('test-collection').document('test')
doc_ref.set({
    'name': current_name,
    'created_at': firestore.SERVER_TIMESTAMP,
    'updated_at': firestore.SERVER_TIMESTAMP
})
```

### `get()`メソッドを使って、指定したドキュメントのデータを読む

```python
db = firestore.Client(project='test-project')
doc_ref = db.collection('test-collection').document('test')
doc = doc_ref.get()
doc_dict = doc.to_dict()
for k in doc_dict.keys():
    print(f'{k} => {doc_dict[k]}')
```

### `update()`メソッドを使って、指定したドキュメントのデータを更新する

```python
db = firestore.Client(project='test-project')
doc_ref = db.collection('test-collection').document('test')
doc_ref.update({
    'name2': 'abcdefg123456',
    'updated_at': firestore.SERVER_TIMESTAMP
    # 新規項目は追加される。
    # 既存項目は更新される。
    # 登場しない項目はそのまま。
})
```

### `where()`メソッドを使って、条件にマッチしたドキュメントにアクセスする

```python
db = firestore.Client(project='test-project')
coll_ref = db.collection('test-collection')
docs = coll_ref.where('name', '==', current_name).stream()
i = 1
for doc in docs:
    print(f'document #{i}---------')
    doc_dict = doc.to_dict()
    for k in doc_dict.keys():
        print(f'{k} => {doc_dict[k]}')
    print(f'----------------------')
    i += 1
```

上のサンプルコードにおける`docs`はジェネレーターであって、コレクションではない。\
吐き出させる以外に、条件にマッチしたドキュメントが全部でいくつあるか知る方法がない

```python
db = firestore.Client(project='amachan')
coll_ref = db.collection(collection_name)
docs = coll_ref.where('willFollow', '==', True).where(
    'didFollow', '==', True).limit(1).stream()

links = []
for doc in docs:
    links.append(doc.to_dict())

print(len(links))
```
