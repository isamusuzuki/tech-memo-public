# Cloud Firestore を使いこなす

作成日 2019/11/13

## 01. コレクションに何件ドキュメントがあるか知る

[Firestore でコレクションの要素数をカウントする方法](https://www.sukerou.com/2019/08/firestore.html)

> 100 件程度の小さいコレクションならば、ローカルでカウントする
>
> 1000 件程度の少し量が多いコレクションならば、Cloud Functions でカウントするようにして、クライアントでキックする
>
> 件数が 1 万件を超えるようなデータ数が多いコレクションならば、データが登録されるタイミングで件数をカウントする

## 02. ドキュメントに書き込むメソッドの違い

Python ドキュメントの DocumentReference 解説 => [https://googleapis.dev/python/firestore/latest/document.html](https://googleapis.dev/python/firestore/latest/document.html)

- `create()` ... Create the current document in the Firestore database.
- `set()` ... Replace the current document in the Firestore database.
- `update()` ... Update an existing document in the Firestore database.

create()メソッドは、すでにドキュメントがある場合はエラーになる。\
set()メソッドは、ドキュメントがあろうがなかろうが関係なく、全項目を上書きする\
update()メソッドは、更新する前に指定したドキュメントの存在をチェックする。指定された項目だけを上書きする

## 03. 複数の条件でクエリーする

たとえば、color という項目があったときに、「red と orange と pink だけを検索する」ということが可能なのか？

[The Firebase Blog: Cloud Firestore Now Supports IN Queries\!](https://firebase.googleblog.com/2019/11/cloud-firestore-now-supports-in-queries.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+FirebaseBlog+%28The+Firebase+Blog%29)

in クエリーの使い方

```javascript
collection("orders").where("status", "in", [
  "Ready to Ship",
  "Out for Delivery",
  "Delivered"
]);
```

array-contains-any クエリーの使い方

```javascript
collection("products").where("category", "array-contains-any", [
  "Appliances",
  "Electronics"
]);
```

## 04. タイムスタンプが JSON ダンプできずにエラーを吐き出す件

### エラーが起こるスクリプト

```python
import json
from google.cloud import firestore


db = firestore.Client(project='test-project')
coll_ref = db.collection('test-collection')
docs = coll_ref.where('isEnd', '==', True).where(
    'didPick', '==', True).limit(1).stream()

links = []
for doc in docs:
    links.append(doc.to_dict())

jsonstr = json.dumps(links[0])
print(jsonstr)
```

=> `TypeError: Object of type DatetimeWithNanoseconds is not JSON serializable`

### エラーの理由

タイムスタンプは、`DatetimeWithNanoseconds`クラスのインスタンスである

```python
from google.api_core.datetime_helpers import DatetimeWithNanoseconds

updated_at = links[0]['updated_at']

print(type(updated_at))
# => <class 'google.api_core.datetime_helpers.DatetimeWithNanoseconds'>

print(isinstance(updated_at, DatetimeWithNanoseconds))
# => True
```

### 解決したスクリプト

`json.dumps()`に、default オプションを使って、JSON 対象外の型も JSON 文字列化できるようにする

```python
import json

from google.api_core.datetime_helpers import DatetimeWithNanoseconds
from google.cloud import firestore

db = firestore.Client(project='test-project')
coll_ref = db.collection('test-collection')
docs = coll_ref.where('isEnd', '==', True).where(
    'didPick', '==', True).limit(1).stream()

links = []

for doc in docs:
    links.append(doc.to_dict())

def helper(obj):
    if isinstance(obj, DatetimeWithNanoseconds):
        return obj.isoformat()
    raise TypeError (f'Type {type(obj)} not serializable')

jsonstr = json.dumps(links[0], default=helper)
print(jsonstr)
```

## 05. Cloud Firestore にインデックスを追加する

### 問題発生

```python
db = firestore.Client(project='test-project')
coll_ref = db.collection('test-collection')
docs = coll_ref.where('didSearch', '==', True).where(
    'are_they_here', '==', False).where(
    'is_nobody_here', '==', False).where(
    'updated_at', '>=', start).where(
    'updated_at', '<', end).stream()
```

where 句で不等号を使うと、`google.api_core.exceptions.FailedPrecondition: 400 The query requires an index`というエラーが起こる

### 解決方法

GCP コンソール　＞ Firestore ＞ 左枠 ＞ インデックスをクリック\
COMPOSITE タグをクリック ＞ 「複合インデックス」ページ\
「インデックスを作成」ボタンをクリック

| Key                                | Value               |
| ---------------------------------- | ------------------- |
| コレクション ID                    | test-collection     |
| インデックスを作成するフィールド 1 | didSearch 昇順      |
| インデックスを作成するフィールド 2 | are_they_here 昇順  |
| インデックスを作成するフィールド 3 | is_nobody_here 昇順 |
| インデックスを作成するフィールド 4 | updated_at 昇順     |
| クエリーの範囲                     | コレクション        |

フィールドの順番は大事。まったく同じ順番にしておかないと機能しない

## 06. タイムスタンプを where クエリーで使う

タイムスタンプ項目には、DatetimeWithNanoseconds クラスが入っているのだから、\
こちらも、文字列ではなく、datetime クラスを比較対象として使う

```python
import datetime

import pytz

end = datetime.datetime.now(pytz.timezone('UTC'))
start = end - datetime.timedelta(days=1)

db = firestore.Client(project='test-project')
coll_ref = db.collection(f'test-collection')
docs = coll_ref.where('didSearch', '==', True).where(
    'are_they_here', '==', False).where(
    'is_nobody_here', '==', False).where(
    'updated_at', '>=', start).where(
    'updated_at', '<', end).stream()
```
