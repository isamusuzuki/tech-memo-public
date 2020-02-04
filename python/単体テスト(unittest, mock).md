---
tags: python
---

# 単体テスト(unittest, mock)

作成日 2019/11/30

## 01. unittestのサンプルコード

ディレクトリ構造

```=
--avocado/
    |--apps/
    |   |--__init__.py
    |   `--util.py
    |
    `--tests/
        |--__init__.py
        `--test_util.py
```

`tests/test_util.py`ファイル

```python
import unittest
from apps.util import getParams


class TestUtil(unittest.TestCase):
    def test_getParams(self):
        data = {}
        result = getParams(data)
        self.assertTrue(result['gotParams'])


if __name__ == '__main__':
    unittest.main()
```

単体テストの実行

```bash=
cd avocado
unittest discover tests
```

## 02. mockのサンプルコード

### mockの使い方

mockは、1つの関数に対して、1つのオブジェクトだけを偽装する\
つまり、1つの関数は、1つのオブジェクトだけを持つべきである\
そもそも、2つ以上のオブジェクトに依存している関数を書いてはいけない

### mockで環境変数を置き換える

tests/test_util.py

```python=
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

### mockでurlrequestを置き換える

まずはpatchで、urllib.request.urlopenメソッドを偽装する\
urlopenは実行されているのだから、`.return_value`をつける\
それからwith構文の中に入っていっているから、`.__enter__`をつける\
それも実は関数なのだから、`.return_value`をつける\
これでwith構文内で使われるresを偽装できる\
resは`read()`を実行し、その戻り値が`decode()`を実行している
`mock_res.read.return_value.decode.return_value`で
bodyを偽装できる

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

```python=
with Foo('foo') as f:
  print f.xxx()
```

with文に入ったときに、`__enter__()`メソッドが呼ばれる\
with文を抜けるときに、`__exit__()`メソッドが呼ばれる

上のwith文をMockで偽装するときは、以下のように書く

```python=
from unittest.mock import MagikMock, patch


@patch('Foo')
def test_Foo(self, mock_foo):
    mock_f = MagicMock()
    mock_f.xxx.return_value = 'fuga'
    
    mock_foo.return_value.__enter__.return_value = mock_f
```

### mockでMySQL connectを置き換える

[Mock a MySQL database in Python \- Stack Overflow](https://stackoverflow.com/questions/28431452/mock-a-mysql-database-in-python)
