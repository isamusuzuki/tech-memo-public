# 単体テストで mock を使う

作成日 2019/11/30、更新日 2022/08/30

## mock の使い方

mock は、1 つの関数に対して、1 つのオブジェクトだけを偽装する\
つまり、1 つの関数は、1 つのオブジェクトだけを持つべきである\
そもそも、2 つ以上のオブジェクトに依存している関数を書いてはいけない

## mock で環境変数を置き換える

tests/test_util.py

```python
import os
import unittest
from unittest import mock
from apps.util import getKey


class TestUtil(unittest.TestCase):
    @mock.patch.dict(os.environ, {
        'test_license_key': 'aabbcc',
        'test_service_secret': 'ddeeff'}
    )
    def test_getKey(self):
        result = getKey('staging', '101')
        self.assertTrue(result['gotKey'])
        self.assertEqual(result['LICENSE_KEY'], 'aabbcc')
        self.assertEqual(result['SERVICE_SECRET'], 'ddeeff')


if __name__ == '__main__':
    unittest.main()
```

## mock で urlrequest を置き換える

まずは patch で、urllib.request.urlopen メソッドを偽装する\
urlopen は実行されているのだから、`.return_value`をつける\
それから with 構文の中に入っていっているから、`.__enter__`をつける\
それも実は関数なのだから、`.return_value`をつける\
これで with 構文内で使われる res を偽装できる\
res は`read()`を実行し、その戻り値が`decode()`を実行している
`mock_res.read.return_value.decode.return_value`で
body を偽装できる

`main.py`ファイル

```python
import urllib.request
import json


def get_data():
    url = 'https://jsonplaceholder.typicode.com/todos/1'
    mehtod = 'GET'
    req = urllib.request.Request(url, method=mehtod)
    try:
        with urllib.request.urlopen(req) as res:
            body = res.read().decode('utf-8')
            body_dict = json.loads(body)
            return body_dict
    except urllib.error.HTTPError as err:
        print(err.code)
        print(err.reason)
    except urllib.error.URLError as err:
        print(err.reason)
if __name__ == '__main__':
    data = get_data()
    print(data)
```

`tests/test_main.py`ファイル

```python
import unittest
from unittest.mock import MagicMock, patch
from main import get_data


class TestTest(unittest.TestCase):
    @patch('urllib.request.urlopen')
    def test_main(self, mock_urlopen):
        mock_res = MagicMock()
        mock_res.read.return_value.decode.return_value = '{"id": 1}'
        mock_urlopen.return_value.__enter__.return_value = mock_res
        data_dict = get_data()
        self.assertEqual(data_dict['id'], 1)
```

### `__enter__()`と`__exit__()`とは

```python
with Foo('foo') as f:
  print f.xxx()
```

with 文に入ったときに、`__enter__()`メソッドが呼ばれる\
with 文を抜けるときに、`__exit__()`メソッドが呼ばれる

上の with 文を Mock で偽装するときは、以下のように書く

```python
from unittest.mock import MagikMock, patch


@patch('Foo')
def test_Foo(self, mock_foo):
    mock_f = MagicMock()
    mock_f.xxx.return_value = 'fuga'

    mock_foo.return_value.__enter__.return_value = mock_f
```

## mock で MySQL connect を置き換える

[Mock a MySQL database in Python \- Stack Overflow](https://stackoverflow.com/questions/28431452/mock-a-mysql-database-in-python)
