# CROSSMALL APIを利用する

作成日 2019/11/20

## 注文データを取得する

get_order.py

```python
import hashlib
import os
import urllib.parse
import urllib.request
import xml.etree.ElementTree as ET

# クロスモールAPI テスト店舗用接続情報
CROSSMALL_TEST_AUTHENTICATION_KEY = os.environ.get(
    'CROSSMALL_TEST_AUTHENTICATION_KEY')
CROSSMALL_TEST_ACCOUNT = os.environ.get('CROSSMALL_TEST_ACCOUNT')
PHASE_NAME = urllib.parse.quote('クレジット・YJ・午前')

url = 'https://www.crossmall.jp/webapi2/get_order'
params1 = f'account={CROSSMALL_TEST_ACCOUNT}&phase_name={PHASE_NAME}'
params2 = params1 + CROSSMALL_TEST_AUTHENTICATION_KEY
signing = hashlib.md5(params2.encode('utf-8')).hexdigest()
params3 = f'{params1}&signing={signing}'

req = urllib.request.Request(f'{url}?{params3}')

try:
    with urllib.request.urlopen(req) as res:
        xml_data = res.read().decode('utf-8')
        root = ET.fromstring(xml_data)
        results = root.findall('./ResultSet/Result')
        i = 1
        for result in results:
            txt = f'{i} {result.findtext("order_date")}: '
            txt += f'{result.findtext("order_number")} => '
            txt += f'{result.findtext("phase_name")}'
            print(txt)
            i += 1
except urllib.error.HTTPError as err:
    print(f'ERROR => {err.code} {err.reason}')
    xml_data = err.read().decode('utf-8')
    print(xml_data)
except urllib.error.URLError as err:
    print(f'ERROR => {err.reason}')
```

## 商品属性情報を取得する

get_item_attr.py

```python
import hashlib
import os
import urllib.error
import urllib.parse
import urllib.request
import xml.etree.ElementTree as ET

# クロスモールAPI テスト店舗用接続情報
CROSSMALL_TEST_AUTHENTICATION_KEY = os.environ.get(
    'CROSSMALL_TEST_AUTHENTICATION_KEY')
CROSSMALL_TEST_ACCOUNT = os.environ.get('CROSSMALL_TEST_ACCOUNT')
ITEM_CODE = 'zip822'
ATTRIBUTE_TYPE = '2'

url = 'https://www.crossmall.jp/webapi2/get_item_attribute'

params1 = (
    f'account={CROSSMALL_TEST_ACCOUNT}&item_code={ITEM_CODE}'
    f'&attribute_type={ATTRIBUTE_TYPE}'
)
params2 = params1 + CROSSMALL_TEST_AUTHENTICATION_KEY
signing = hashlib.md5(params2.encode('utf-8')).hexdigest()
params3 = f'{params1}&signing={signing}'

req = urllib.request.Request(f'{url}?{params3}')

try:
    with urllib.request.urlopen(req) as res:
        xml_data = res.read().decode('utf-8')
        root = ET.fromstring(xml_data)
        results = root.findall('./ResultSet/Result')
        i = 1
        for result in results:
            txt = f'{i} {result.findtext("attribute_name")}'
            print(txt)
            i += 1
except urllib.error.HTTPError as err:
    print(f'ERROR => {err.code} {err.reason}')
    xml_data = err.read().decode('utf-8')
    print(xml_data)
except urllib.error.URLError as err:
    print(f'ERROR => {err.reason}')
```

