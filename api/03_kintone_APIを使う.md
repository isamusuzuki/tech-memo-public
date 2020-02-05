# kintone API を使う

作成日 2020/02/03、更新日 2020/02/05

## 01. データを読む

ドキュメント => [レコードの取得（GET） – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/202331474)

> レコードの一括取得（クエリで条件を指定）\
> 
> 一度に取得できるレコードは 500件までです。

```python
import datetime
import json
import os
import urllib.parse
import urllib.request

import pytz

KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')

TODAY = datetime.datetime.now(pytz.timezone('Asia/Tokyo'))
TODAY_STR = TODAY.strftime("%Y-%m-%d")

base_url = 'https://abcdefg.cybozu.com/k/v1/records.json'
params = {
    'app': 'nn',
    'query': (
        'imageCompletion_date >= "1900-01-01"'
        f' and imageCompletion_date <= "{TODAY_STR}"'
        ' and image_folder_status !=  "OK"'
        ' and status in ("通常")'
        ' order by kanri_no'
        f' limit {limit}'
    ),
    'fields[0]': 'kanri_no',
    'fields[1]': 't_number',
    'fields[2]': 'item_code',
    'fields[3]': 'image_folder_status',
    'fields[4]': 'imageCompletion_date',
    'TotalCount': False
}
headers = {
    'X-Cybozu-API-Token': KINTONE_API_TOKEN
}
url = f'{base_url}?{urllib.parse.urlencode(params)}'
req = urllib.request.Request(url, headers=headers)

try:
    with urllib.request.urlopen(req) as res:
        body = res.read().decode('utf-8')
        result_dict = json.loads(body)
    records = []
    for r in result_dict['records']:
        records.append({
            'kanri_no': int(r['kanri_no']['value']),
            't_number': r['t_number']['value'],
            'item_code': r['item_code']['value'],
            'imageCompletion_date': r['imageCompletion_date']['value'],
            'image_folder_status': r['image_folder_status']['value']
        })
    response = {'success': True, 'records': records}
except urllib.error.HTTPError as err:
    response = {'success': False,
                'message': f'ERROR => {err.code} {err.reason}'}
except urllib.error.URLError as err:
    response = {'success': False, 'message': f'ERROR => {err.reason}'}
finally:
    return response
```

## 02. データを更新する

ドキュメント => [レコードの更新（PUT） – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/201941784)

> レコードの一括更新\
>
> 一度に更新できるレコードは 100 件までです。\
> 一括更新に失敗すると、リクエストに含まれるレコードの処理はすべてキャンセルされます。

```python
import json
import os
import urllib.error
import urllib.request


KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')

records = []
for id in id_list:
    records.append({
        'id': id,
        'record': {
            'image_folder_status': {
                'value': 'OK'
            }
        }
    })

url = 'https://abcdefg.cybozu.com/k/v1/records.json'
method = 'PUT'
data = {
    'app': '38',
    'records': records
}
json_data = json.dumps(data).encode('utf-8')

headers = {
    'X-Cybozu-API-Token': KINTONE_API_TOKEN,
    'Content-Type': 'application/json; charset=utf-8'
}
req = urllib.request.Request(
    url, data=json_data, method=method, headers=headers)

try:
    with urllib.request.urlopen(req) as res:
        _ = res.read().decode('utf-8')
    response = {'success': True}
except urllib.error.HTTPError as err:
    response = {'success': False,
                'message': f'ERROR => {err.code} {err.reason}'}
except urllib.error.URLError as err:
    response = {'success': False, 'message': f'ERROR => {err.reason}'}
finally:
    return response
```

## 03. 認証に関する注意点

ローカルでテストしていた時は、何の問題もなかったのに、\
Cloud Functions にデプロイした途端に`401 Unauthorized`ではじかれた\
「アクセスするには認証が必要です」とのこと

kintone API は、IP アドレス制限がかかっている。\
社内ネットワークのグルーバル IP は、登録済みだが、\
それ以外からのアクセスの場合は、API トークンに加えて、\
ベーシック認証を必要とする

### API トークンとベーシック認証を使ったコード

```python
import base64

KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')
KINTONE_BASIC_USERNAME = os.environ.get('KINTONE_BASIC_USERNAME')
KINTONE_BASIC_PASSWORD = os.environ.get('KINTONE_BASIC_PASSWORD')

a = f'{KINTONE_BASIC_USERNAME}:{KINTONE_BASIC_PASSWORD}'
b = base64.b64encode(a.encode('utf-8'))
self.basic_account = f'Basic {b.decode("utf-8")}'

headers = {
    'Content-Type': 'application/json',
    'Authorization': self.basic_account,
    'X-Cybozu-API-Token': KINTONE_API_TOKEN
}
```

