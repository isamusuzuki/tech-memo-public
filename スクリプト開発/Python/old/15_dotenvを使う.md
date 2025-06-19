# dotenv を使う

作成日 2020/03/26

## 01. dotenv とは

環境変数にキーバリューペアを追加する

公式トップ => [python\-dotenv](https://saurabh-kumar.com/python-dotenv/)

インストール => `pip install python-dotenv`

python-dotenv は、.env ファイルに書いておいたキーバリューペアを環境変数に追加する\
.env ファイルは、`API_KEY=XXXXXXXX`と書く

.env ファイルは、同じフォルダにあるものとする

```python
from dotenv import load_dotenv

load_dotenv()
```

## 02. 環境変数を読み込む

`load_dotenv()` が実行されるまで、環境変数にはなにも入っていないので要注意\
メソッドやクラスの中で、環境変数を読むことを習慣にする

```python
import os

class CVG():
    def __init__(self, env='local'):
        CVG_HOST_NAME = os.environ.get('CVG_HOST_NAME')
        CVG_USER_NAME = os.environ.get('CVG_USER_NAME')
        CVG_PASSWORD = os.environ.get('CVG_PASSWORD')
        CVG_DB_NAME = os.environ.get('CVG_DB_NAME')
```
