# Amazon MWS APIを勉強する

作成日 2019/11/12

## 01. Amazon MWS APIにアクセスする

### つまづきやすいポイント

- パラメーターをurlencodeする前に、ソートすること
- urlencodeするときは、quote方式を使う。デフォルトではquote_plus方式なので、指定する必要がある
- POSTメソッドで送信するけれども、GETパラメーターを使う。したがってボディは空
- ボディは空だけれども、Content-Typeはtext/xmlを指定する

### サンプルコード

```python
import base64
import datetime
import hashlib
import hmac
import os
import urllib.error
import urllib.parse
import urllib.request

MWS_SELLER_ID = os.environ.get('MWS_SELLER_ID')
MWS_ACCESS_KEY_ID = os.environ.get('MWS_ACCESS_KEY_ID')
MWS_ACCESS_SECRET = os.environ.get('MWS_ACCESS_SECRET')
MWS_MARKETPLACE_ID = os.environ.get('MWS_MARKETPLACE_ID')

DOMAIN = 'mws.amazonservices.jp'
ENDPOINT = '/Products/2011-10-01'
METHOD = 'POST'


def products1(query):
    timestamp = datetime.datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%SZ')
    params = {
        'Action': 'ListMatchingProducts',
        'Query': query,
        'AWSAccessKeyId': MWS_ACCESS_KEY_ID,
        'MarketplaceId': MWS_MARKETPLACE_ID,
        'SellerId': MWS_SELLER_ID,
        'SignatureMethod': 'HmacSHA256',
        'SignatureVersion': '2',
        'Timestamp': timestamp,
        'Version': '2011-10-01'
    }
    params_string = urllib.parse.urlencode(
        sorted(params.items()), quote_via=urllib.parse.quote)

    canonical = (
        f'{METHOD}\n'
        f'{DOMAIN}\n'
        f'{ENDPOINT}\n'
        f'{params_string}'
    )

    h = hmac.new(
        MWS_ACCESS_SECRET.encode('utf-8'),
        canonical.encode('utf-8'),
        hashlib.sha256
    )
    params['Signature'] = base64.b64encode(h.digest())

    query_string = urllib.parse.urlencode(
        sorted(params.items()), quote_via=urllib.parse.quote)
    url = f'https://{DOMAIN}{ENDPOINT}?{query_string}'

    headers = {
        'Content-Type': 'text/xml'
    }
    req = urllib.request.Request(
        url, method=METHOD, headers=headers
    )
    try:
        with urllib.request.urlopen(req) as res:
            body = res.read().decode('utf-8')
        print('urllib => success')
    except urllib.error.HTTPError as err:
        print(f'HTTPError => {err.code}:{err.reason}')
        body = err.read().decode('utf-8')
    except urllib.error.URLError as err:
        print(f'URLError => {err.reason}')
        body = ''
    finally:
        if body:
            with open('../temp/products1.txt',
                      mode='w', encoding='utf-8') as f:
                f.write(body)


if __name__ == "__main__":
    products1('Python データ分析')
```

## 02. 名前空間付きXMLデータを解読する

### Amazon MWS APIの戻りデータの例

```xml
<?xml version="1.0"?>
<ListMatchingProductsResponse xmlns="http://mws.amazonservices.com/schema/Products/2011-10-01">
<ListMatchingProductsResult>
<Products xmlns:ns2="http://mws.amazonservices.com/schema/Products/2011-10-01/default.xsd">
  <Product>
    <Identifiers/>
  </Product>
</Products>
</ListMatchingProductsResult>
<ResponseMetadata>
  <RequestId>197b9e59-f4d6-4af3-a620-ff6ca4d08d39</RequestId>
</ResponseMetadata>
</ListMatchingProductsResponse>
```

### つまづきやすいポイント

名前空間は指定しないと、すべての頭についてきて、とても煩わしい

```python
from lxml import etree

tree = etree.parse('../temp/products1.txt')
root = tree.getroot()
print(root.tag)
#=> {http://mws.amazonservices.com/schema/Products/2011-10-01}ListMatchingProductsResponse

for child in root:
  print(child.tag)
  #=> {http://mws.amazonservices.com/schema/Products/2011-10-01}ListMatchingProductsResult
  #=> {http://mws.amazonservices.com/schema/Products/2011-10-01}ResponseMetadata
```

### サンプルコード

```python
import xml.etree.ElementTree as ET

tree = ET.parse('../temp/products1.txt')
root = tree.getroot()

ns = {
    'ns2': (
        'http://mws.amazonservices.com/schema/Products'
        '/2011-10-01/default.xsd'
    )}
i = 1
for p in root[0][0]:
    group = p.find('.//ns2:ProductGroup', ns)
    publisher = p.find('.//ns2:Publisher', ns)
    title = p.find('.//ns2:Title', ns)
    try:
        print(f'{i} => ({group.text}){publisher.text}「{title.text}」')
    except AttributeError:
        print(f'{i} => タイトルなし')
    finally:
        i += 1
```
