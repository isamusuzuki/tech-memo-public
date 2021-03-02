# Fire を使う

作成日 2020/03/26、更新日 2020/08/12

## 01. Fire とは

Python で簡単に CLI ツールを作成するためのライブラリ

公式トップ => [google/python\-fire: Python Fire is a library for automatically generating command line interfaces \(CLIs\) from absolutely any Python object\.](https://github.com/google/python-fire)

インストール => `pip install fire`

マニュアル => [python\-fire/using\-cli\.md at master · google/python\-fire](https://github.com/google/python-fire/blob/master/docs/using-cli.md)

## 02. Fire の使い方

### メソッドを Fire に登録する

hello.py

```python
from fire import Fire

def hello(name="World"):
  return "Hello %s!" % name

if __name__ == '__main__':
  Fire(hello)
```

hello.py を使う

```bash
python hello.py  # Hello World!
python hello.py --name=David  # Hello David!
python hello.py --help  # 使い方情報を表示する
```

### クラスを Fire に登録する

calculator.py

```python
from fire import Fire

class Calculator(object):
  """A simple calculator class."""

  def double(self, number):
    return 2 * number

  def triple(self, number):
    return 3 * number

if __name__ == '__main__':
  Fire(Calculator)
```

calculator.py を使う

```bash
python calculator.py double 10  # 20
python calculator.py double --number=10  # 20
python calculator.py triple 10  # 30
python calculator.py triple --number=10  # 30
```

クラスを登録すると、複数のメソッドが使えるところがいい
