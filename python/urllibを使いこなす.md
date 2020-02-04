---
tags: python
---

# urllibを使いこなす

作成日 2019/12/03

## 01. サンプルコード

```python=
import json
import urllib

url = 'https://api.exmple.com/order/cancelOrder/'
method = 'POST'
data = {
    'orderNumber': order_code,
    'inventoryRestoreType': 2,
    'changeReasonDetailApply': 1
}
json_data = json.dumps(data).encode('utf-8')
headers = {
    'Authorization': f'ESA {encryptedUserPass}',
    'Content-Type': 'application/json; charset=utf-8'
}
req = urllib.request.Request(
    url, data=json_data, method=method, headers=headers)

result = {'success': False}
try:
    with urllib.request.urlopen(req) as res:
        body = res.read().decode('utf-8')
    body_dict = json.loads(body)
    result['data'] = body_dict
    result['success'] = True
# HTTPステータスコードが4xxまたは5xxだったとき
except urllib.error.HTTPError as err:
    body = err.read().decode('utf-8')
    body_dict = json.loads(body)
    result['data'] = body_dict
    result['message'] = f'HTTPError => {err.code}:{err.reason}'
# HTTP通信に失敗したとき
except urllib.error.URLError as err:
    result['message'] = f'URLError => {err.reason}'
finally:
    return result
```

### 覚えておくべきこと

urllib.error.HTTPErrorモジュールは、read()メソッドを持っている\
したがって、except構文の中でも、返事の内容を取得することが可能

## 02. urllib.parseを使うときの注意点

公式ドキュメント => [https://docs.python.org/ja/3/library/urllib.parse.html](https://docs.python.org/ja/3/library/urllib.parse.html)

- `urllib.parse.quote(string)` ... URL構成要素として使えるように、文字列をクオートする
- `urllib.parse.urlencode(dict)` ... &文字で区切られた`key=value`からなるクォート文字列を作成する

```python=
import urllib.parse

txt = 'Python データ分析'
print(f'Query={urllib.parse.quote(txt)}')
# => Query=Python%20%E3%83%87%E3%83%BC%E3%82%BF%E5%88%86%E6%9E%90

dct = {'Query': 'Python データ分析'}
print(urllib.parse.urlencode(dct))
# => Query=Python+%E3%83%87%E3%83%BC%E3%82%BF%E5%88%86%E6%9E%90
```

この2つは、クォート方法が**同じ**ではない！

`urllib.parse.quote_plus(string)`というメソッドも存在し\
`urllib.parse.urlencode()`メソッドは、内部でそちらを使う

| メソッド       | 空白 | スラッシュ |
| -------------- | ------- | -------------- |
| `quote()`      | `%20`   | 変換しない     |
| `quote_plus()` | `+`     | `%2F`          |

```python=
txt = 'Python データ分析'
print(f'Query={urllib.parse.quote_plus(txt)}')
# => Query=Python+%E3%83%87%E3%83%BC%E3%82%BF%E5%88%86%E6%9E%90
```

`urlencode()`メソッドを使いつつ、`quote()`メソッドのクォート方法を\
採用したい場合は、`quote_via`引数を指定する

```python=
dct = {'Query': 'Python データ分析'}
print(urllib.parse.urlencode(dct, quote_via=urllib.parse.quote))
# => Query=Python%20%E3%83%87%E3%83%BC%E3%82%BF%E5%88%86%E6%9E%90
```

## 03. urllib.request.urlopenの戻り値について

ドキュメント => [urllib\.request \-\-\- URL を開くための拡張可能なライブラリ — Python 3\.8\.0 ドキュメント](https://docs.python.org/ja/3/library/urllib.request.html)

> urllib.request.urlopen関数は常に コンテキストマネージャとして\
> 機能するオブジェクトを返します。\
> このオブジェクトには以下のメソッドがあります。
>
> - geturl() ... 取得されたリソースのURLを返します
> - info() ... http.client.HTTPMessageオブジェクトを返す
> - getcode() ... レスポンスのHTTPステータスコード

http.client.HTTPMessageオブジェクトは、email.message.Messageクラスを利用している

[email\.message\.Message: Representing an email message using the compat32 API — Python 3\.8\.0 ドキュメント](https://docs.python.org/ja/3/library/email.compat32-message.html#email.message.Message)

get_content_charset(failobj) ... Content-Typeヘッダにある、charsetパラメータの値を返します。値はすべて小文字に変換されます。メッセージ中にContent-Typeがなかったり、このヘッダ中にcharasetパラメータがない場合にはfailobjが返されます。

戻りデータの文字コードがわからない場合の書き方

```python=
import urllib.request

with urllib.request.urlopen(req) as res:
    encoding = res.info().get_content_charset(failobj='utf-8')
    print(encoding)
    result['data'] = res.read().decode(encoding)
```
