# 楽天 RMS API を利用する

作成日 2019/11/20

## 01. 注文情報を取得する

```python
import base64
import json
import os
import sys
import urllib.error
import urllib.request

# 引数が必要
try:
    orderNumber = sys.argv[1]
except IndexError:
    print('!!!エラー!!! 注文番号を入力してください')
    sys.exit()

# 環境変数が必要
serviceSecret = os.environ.get('RMS_TEST_SERVICE_SECRET')
licenseKey = os.environ.get('RMS_TEST_LICENSE_KEY')

userPass = f'{serviceSecret}:{licenseKey}'
encryptedUserPass = base64.b64encode(userPass.encode('utf-8')).decode('utf-8')

url = 'https://api.rms.rakuten.co.jp/es/2.0/order/getOrder/'
method = 'POST'
data = {
    'orderNumberList': [orderNumber]
}
json_data = json.dumps(data).encode('utf-8')
headers = {
    'Authorization': f'ESA {encryptedUserPass}',
    'Content-Type': 'application/json; charset=utf-8'
}
req = urllib.request.Request(
    url, data=json_data, method=method, headers=headers)

try:
    with urllib.request.urlopen(req) as res:
        body = res.read().decode('utf-8')
        result_obj = json.loads(body)
        if len(result_obj['OrderModelList']) == 1:
            thisOrder = result_obj['OrderModelList'][0]
            print(f'orderNumber = {thisOrder["orderNumber"]}')
            print(f'orderProgress = {thisOrder["orderProgress"]}')
            print(f'orderDateTime = {thisOrder["orderDatetime"]}')

# HTTPステータスコードが4xxまたは5xxだったとき
except urllib.error.HTTPError as err:
    print(f'error! {err.code} {err.reason}')

# HTTP通信に失敗したとき
except urllib.error.URLError as err:
    print(f'error! {err.reason}')
```

## 02. 注文をキャンセルする

```python
import base64
import json
import os
import sys
import urllib.error
import urllib.request

# 引数が必要
try:
    orderNumber = sys.argv[1]
except IndexError:
    print('!!!エラー!!! 注文番号を入力してください')
    sys.exit()

# 環境変数が必要
serviceSecret = os.environ.get('RMS_TEST_SERVICE_SECRET')
licenseKey = os.environ.get('RMS_TEST_LICENSE_KEY')

userPass = f'{serviceSecret}:{licenseKey}'
encUserPass = base64.b64encode(userPass.encode('utf-8')).decode('utf-8')

url = 'https://api.rms.rakuten.co.jp/es/2.0/order/cancelOrder/'
method = 'POST'
data = {
    'orderNumber': orderNumber,
    'inventoryRestoreType': 2,
    'changeReasonDetailApply': 1
}
json_data = json.dumps(data).encode('utf-8')
headers = {
    'Authorization': f'ESA {encUserPass}',
    'Content-Type': 'application/json; charset=utf-8'
}
req = urllib.request.Request(
    url, data=json_data, method=method, headers=headers)

try:
    with urllib.request.urlopen(req) as res:
        body = res.read().decode('utf-8')
        result_obj = json.loads(body)
        if len(result_obj['MessageModelList']) == 1:
            thisOrder = result_obj['MessageModelList'][0]
            print(f'orderNumber = {thisOrder["orderNumber"]}')
            print(f'messageType = {thisOrder["messageType"]}')
            print(f'messageCode = {thisOrder["messageCode"]}')
            print(f'message = {thisOrder["message"]}')

# HTTPステータスコードが4xxまたは5xxだったとき
except urllib.error.HTTPError as err:
    print(f'error! {err.code} {err.reason}')
    body = err.read().decode('utf-8')
    result_obj = json.loads(body)
    if len(result_obj['MessageModelList']) == 1:
        thisOrder = result_obj['MessageModelList'][0]
        print(f'orderNumber = {thisOrder["orderNumber"]}')
        print(f'messageType = {thisOrder["messageType"]}')
        print(f'messageCode = {thisOrder["messageCode"]}')
        print(f'message = {thisOrder["message"]}')

# HTTP通信に失敗したとき
except urllib.error.URLError as err:
    print(f'error! {err.reason}')
```
