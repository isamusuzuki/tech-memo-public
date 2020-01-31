# 出品者APIを使う

作成日 2019/11/22

## ListMarketplaceParticipations

seller.py - もっとも簡単なAPI

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

DOMAIN = 'mws.amazonservices.jp'
ENDPOINT = '/Sellers/2011-07-01'
METHOD = 'POST'
TEMP_FILE = '../temp/sellers.txt'


def sellers():
    timestamp = datetime.datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%SZ')
    params = {
        'Action': 'ListMarketplaceParticipations',
        'AWSAccessKeyId': MWS_ACCESS_KEY_ID,
        'SellerId': MWS_SELLER_ID,
        'SignatureMethod': 'HmacSHA256',
        'SignatureVersion': '2',
        'Timestamp': timestamp,
        'Version': '2011-07-01'
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
            with open(TEMP_FILE,
                      mode='w', encoding='utf-8') as f:
                f.write(body)


if __name__ == "__main__":
    sellers()
```




