# Fire を使いこなす

作成日 2020/03/26

## 01. Fire とは

Python で簡単に CLI ツールを作成するためのライブラリ

公式トップ => [google/python\-fire: Python Fire is a library for automatically generating command line interfaces \(CLIs\) from absolutely any Python object\.](https://github.com/google/python-fire)

インストール => `pip install fire`

マニュアル => [python\-fire/using\-cli\.md at master · google/python\-fire](https://github.com/google/python-fire/blob/master/docs/using-cli.md)

## 02. Fire の使い方

### メソッドを Fire に登録する

hello.py

```python
import fire

def hello(name="World"):
  return "Hello %s!" % name

if __name__ == '__main__':
  fire.Fire(hello)
```

hello.py を使う

```bash
python hello.py  # Hello World!
python hello.py --name=David  # Hello David!
python hello.py --help  # Shows usage information.
```

### クラスを Fire に登録する

calculator.py

```python
import fire

class Calculator(object):
  """A simple calculator class."""

  def double(self, number):
    return 2 * number

if __name__ == '__main__':
  fire.Fire(Calculator)
```

calculator.py を使う

```bash
python calculator.py double 10  # 20
python calculator.py double --number=15  # 30
```

クラスを登録すると、複数のメソッドが使えるところがいい
