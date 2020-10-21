# kintone API を使う

作成日 2020/02/03、更新日 2020/10/21

## 01. データを取得する

ドキュメント => [レコードの取得（GET） – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/202331474)

- 一度に取得できるレコードは 500 件まで

```python
import base64
import datetime
import json
import os
import urllib.parse
import urllib.request

import pytz

KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')
KINTONE_BASIC_USERNAME = os.environ.get('KINTONE_BASIC_USERNAME')
KINTONE_BASIC_PASSWORD = os.environ.get('KINTONE_BASIC_PASSWORD')

TODAY = datetime.datetime.now(pytz.timezone('Asia/Tokyo'))
TODAY_STR = TODAY.strftime("%Y-%m-%d")
base_url = 'https://abcdefg.cybozu.com/k/v1/records.json'
params = {
    'app': 99,
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

a = f'{KINTONE_BASIC_USERNAME}:{KINTONE_BASIC_PASSWORD}'
b = base64.b64encode(a.encode('utf-8'))
basic_account = f'Basic {b.decode("utf-8")}'
headers = {
    'Authorization': basic_account,
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

## 02. カーソルを使ってデータを取得する

ドキュメント => [レコードの一括取得 – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/360029152012)

- アプリに対して、レコード取得用のカーソルを作成できる

```python
import json
import math
import os
import urllib.parse
import urllib.request


class SampleCursor():

    def __init__(self, size):
        self.token = os.environ.get('KINTONE_SHNCHK_TOKEN')
        self.size = size
        self.base_url = 'https://abcdefg.cybozu.com/k/v1/records/cursor.json'
        self.id = 0
        self.totalCount = 0

    def get_data(self):
        if self.id != 0:
            params = {'id': self.id}
            headers = {
                'X-Cybozu-API-Token': self.token
            }
            url = f'{self.base_url}?{urllib.parse.urlencode(params)}'
            req = urllib.request.Request(url, headers=headers)
            try:
                with urllib.request.urlopen(req) as res:
                    body = res.read().decode('utf-8')
                    result_dict = json.loads(body)
                    response = {
                        'success': True,
                        'message': '',
                        'result': result_dict
                    }
            except urllib.error.HTTPError as err:
                body = err.read().decode('utf-8')
                result_dict = json.loads(body)
                response = {
                    'success': False,
                    'message': f'ERROR => {err.code} {err.reason}',
                    'result': result_dict
                }
            except urllib.error.URLError as err:
                response = {
                    'success': False,
                    'message': f'ERROR => {err.reason}'
                }
            finally:
                return response

    def get_id(self):
        url = self.base_url
        method = 'POST'
        data = {'app': 99,
                'query': (
                    'status not in  ("へび","かめ") '
                    'and type not in ("カラー眼鏡")'
                ),
                'fields': [
                    'kanri_no', 'item_code', 'item_name', 'vendor_code'
                ],
                'size': self.size
                }
        json_data = json.dumps(data).encode('utf-8')
        headers = {
            'Content-Type': 'application/json',
            'X-Cybozu-API-Token': self.token
        }
        req = urllib.request.Request(
            url, data=json_data, method=method, headers=headers)
        try:
            with urllib.request.urlopen(req) as res:
                body = res.read().decode('utf-8')
                result_dict = json.loads(body)
                self.id = result_dict['id']
                self.totalCount = int(result_dict['totalCount'])
                response = {
                    'success': True,
                    'message': '',
                    'result': result_dict
                }
        except urllib.error.HTTPError as err:
            body = err.read().decode('utf-8')
            result_dict = json.loads(body)
            response = {
                'success': False,
                'message': f'ERROR => {err.code} {err.reason}',
                'result': result_dict
            }
        except urllib.error.URLError as err:
            response = {
                'success': False,
                'message': f'ERROR => {err.reason}'
            }
        finally:
            return response


if __name__ == '__main__':
    size = 100
    sc = SampleCursor(size)
    result0 = sc.get_id()
    with open('temp/result0.json', mode='w', encoding='utf-8') as f:
        f.write(json.dumps(result0, indent=2, ensure_ascii=False))

    if result0['success']:
        for i in range(0, math.ceil(sc.totalCount / size)):
            result = sc.get_data()
            with open(f'temp/result{i + 1}.json',
                    mode='w', encoding='utf-8') as f:
                f.write(json.dumps(result, indent=2, ensure_ascii=False))
    else:
        print('error')
```

## 03. データを更新する

ドキュメント => [レコードの更新（PUT） – cybozu developer network](https://developer.cybozu.io/hc/ja/articles/201941784)

一件だけの更新と一括の更新は、URL も与えるデータも違うの要注意

### レコードの更新（1 件）

```python
import base64
import json
import os
import urllib.error
import urllib.request

KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')
KINTONE_BASIC_USERNAME = os.environ.get('KINTONE_BASIC_USERNAME')
KINTONE_BASIC_PASSWORD = os.environ.get('KINTONE_BASIC_PASSWORD')

url = 'https://abcdefg.cybozu.com/k/v1/record.json'
method = 'PUT'
data = {
    'app': 99,
    'id': id,
    'record':  {
        'image_folder_status': {
            'value': 'OK'
        }
    }
}
json_data = json.dumps(data).encode('utf-8')
a = f'{KINTONE_BASIC_USERNAME}:{KINTONE_BASIC_PASSWORD}'
b = base64.b64encode(a.encode('utf-8'))
basic_account = f'Basic {b.decode("utf-8")}'
headers = {
    'Content-Type': 'application/json; charset=utf-8'
    'Authorization': basic_account,
    'X-Cybozu-API-Token': KINTONE_API_TOKEN
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

### レコードの一括更新

- 一度に更新できるレコードは 100 件まで
- 一括更新に失敗すると、リクエストに含まれるレコードの処理はすべてキャンセルされる

```python
import base64
import json
import os
import urllib.error
import urllib.request

KINTONE_API_TOKEN = os.environ.get('KINTONE_API_TOKEN')
KINTONE_BASIC_USERNAME = os.environ.get('KINTONE_BASIC_USERNAME')
KINTONE_BASIC_PASSWORD = os.environ.get('KINTONE_BASIC_PASSWORD')

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
    'app': 99,
    'records': records
}
json_data = json.dumps(data).encode('utf-8')
a = f'{KINTONE_BASIC_USERNAME}:{KINTONE_BASIC_PASSWORD}'
b = base64.b64encode(a.encode('utf-8'))
basic_account = f'Basic {b.decode("utf-8")}'
headers = {
    'Content-Type': 'application/json; charset=utf-8'
    'Authorization': basic_account,
    'X-Cybozu-API-Token': KINTONE_API_TOKEN
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

## 04. その他・ティップス

### HTTP ステータス「520」の意味

HTTP ステータス「520」が返ってきたら、それはバリデーションエラーと思うべき\
通常の 500 番台はサーバーエラーであるが、kintone に限ってはこちらのミスの可能性が高い
