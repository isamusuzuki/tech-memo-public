# 商品APIを使う

作成日 2019/11/22

## ListMatchingProducts

```python:products1.py
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

DOMAIN = 'mws.amazonservices.jp'
ENDPOINT = '/Products/2011-10-01'
MARKETPLACE_ID = 'A1VC38T7YXB528'
METHOD = 'POST'
TEMP_FILE = '../temp/products1.xml'


def products1(query):
    timestamp = datetime.datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%SZ')
    params = {
        'Action': 'ListMatchingProducts',
        'Query': query,
        'AWSAccessKeyId': MWS_ACCESS_KEY_ID,
        'MarketplaceId': MARKETPLACE_ID,
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
    body = None
    try:
        with urllib.request.urlopen(req) as res:
            body = res.read().decode('utf-8')
        print('urllib => success')
    except urllib.error.HTTPError as err:
        print(f'HTTPError => {err.code}:{err.reason}')
        body = err.read().decode('utf-8')
    except urllib.error.URLError as err:
        print(f'URLError => {err.reason}')
    finally:
        if body:
            with open(TEMP_FILE,
                      mode='w', encoding='utf-8') as f:
                f.write(body)


if __name__ == "__main__":
    products1('Python データ分析')
```

出力結果をパースする

```python:xml1.py
import xml.etree.ElementTree as ET

TEMP_FILE = '../temp/products1.xml'

tree = ET.parse(TEMP_FILE)
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

## GetLowestPricedOffersForASIN

要注意：パラメーターは、POSTボディに入れる

```python:products2.py
import base64
import datetime
import hashlib
import hmac
import os
import sys
import urllib.error
import urllib.parse
import urllib.request

MWS_SELLER_ID = os.environ.get('MWS_SELLER_ID')
MWS_ACCESS_KEY_ID = os.environ.get('MWS_ACCESS_KEY_ID')
MWS_ACCESS_SECRET = os.environ.get('MWS_ACCESS_SECRET')

DOMAIN = 'mws.amazonservices.jp'
ENDPOINT = '/Products/2011-10-01'
MARKETPLACE_ID = 'A1VC38T7YXB528'
METHOD = 'POST'
TEMP_FILE = '../temp/products2.xml'

def prodcuts2(asin):
    timestamp = datetime.datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%SZ')
    params = {
        'ASIN': asin,
        'ItemCondition': 'New',
        'Action': 'GetLowestPricedOffersForASIN',
        'AWSAccessKeyId': MWS_ACCESS_KEY_ID,
        'MarketplaceId': MARKETPLACE_ID,
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
    url = f'https://{DOMAIN}{ENDPOINT}'

    headers = {
        'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8'
    }
    req = urllib.request.Request(
        url, method=METHOD, data=query_string.encode('utf-8'), headers=headers
    )
    body = None
    try:
        with urllib.request.urlopen(req) as res:
            body = res.read().decode('utf-8')
        print('urllib => success')
    except urllib.error.HTTPError as err:
        print(f'HTTPError => {err.code}:{err.reason}')
        body = err.read().decode('utf-8')
    except urllib.error.URLError as err:
        print(f'URLError => {err.reason}')
    finally:
        return body


if __name__ == "__main__":
    if len(sys.argv) > 1:
        asin = sys.argv[1]
    else:
        asin = 'B078SZ7GK3'
    result = prodcuts2(asin)
    if result:
        with open(TEMP_FILE,
                  mode='w', encoding='utf-8') as f:
            f.write(result)
            print('tempフォルダに結果ファイルを作成しました')
```

出力結果をパースする

```python:xml2.py
import xml.etree.ElementTree as ET

TEMP_FILE = '../temp/products2.xml'

tree = ET.parse(TEMP_FILE)
root = tree.getroot()
summary = root[0][1]
offers = root[0][2]

# セラーの数を調べる
number_of_offers = summary[1]
fba_count = 0
mkt_count = 0
for count in number_of_offers:
    if count.attrib['condition'] in ('new', 'New', 'NEW'):
        if count.attrib['fulfillmentChannel'] == 'Amazon':
            fba_count = int(count.text)
        if count.attrib['fulfillmentChannel'] == 'Merchant':
            mkt_count = int(count.text)
print(f'fba_count => {fba_count}')
print(f'mkt_count => {mkt_count}')

# カートボックスの価格を調べる
buybox_prices = summary[3]
landed_price = 0.0
listing_price = 0.0
shipping = 0.0
points = 0.0
for price in buybox_prices:
    if price.attrib['condition'] in ('new', 'New', 'NEW'):
        landed_price = float(price[0][1].text)
        listing_price = float(price[1][1].text)
        shipping = float(price[2][1].text)
        points = float(price[3][0].text)
print(f'landed_price => {landed_price}')
print(f'listing_price => {listing_price}')
print(f'shipping => {shipping}')
print(f'points => {points}')

# Amazonの存在を調べる
is_amazon_here = False
for offer in offers:
    feedback_rating = offer[1][0].text
    feedback_count = offer[1][1].text
    if feedback_rating == '0.0' and feedback_count == '0':
        is_amazon_here = True
        break
print(f'is_amazon_here => {is_amazon_here}')
```


