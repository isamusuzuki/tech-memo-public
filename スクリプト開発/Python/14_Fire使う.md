# fireを使う

作成日 2020/03/26、更新日 2025/06/19

## 1. fireとは

Pythonで簡単にCLIツールを作成するためのライブラリ

ソースコード => [google/python\-fire: Python Fire is a library for automatically generating command line interfaces \(CLIs\) from absolutely any Python object\.](https://github.com/google/python-fire)

インストール => `pip install fire`

マニュアル => [python\-fire/using\-cli\.md at master · google/python\-fire](https://github.com/google/python-fire/blob/master/docs/using-cli.md)

## 2. fireの使い方

### 2a. メソッドをfireに登録する

hello.py

```python
import fire

def hello(name="World"):
  return "Hello %s!" % name

if __name__ == '__main__':
  fire.Fire(hello)
```

hello.pyを使う

```bash
python hello.py  # Hello World!
python hello.py --name=David  # Hello David!
python hello.py --help  # 使い方情報を表示する
```

### 2b. クラスをfireに登録する

calculator.py

```python
import fire

class Calculator(object):
  """A simple calculator class."""

  def double(self, number):
    return 2 * number

  def triple(self, number):
    return 3 * number

if __name__ == '__main__':
  fire.Fire(Calculator)
```

calculator.pyを使う

```bash
python calculator.py double 10  # 20
python calculator.py double --number=10  # 20
python calculator.py triple 10  # 30
python calculator.py triple --number=10  # 30
```

クラスを登録すると、複数のメソッドが使えるところがいい

## 3. fireの高度な使い方

### 3a. カンマ区切りの文字列を与える

run1.py

```python
import fire

class Run():
  def job1(self, item_codes: str) -> None:
    print(f'item_codes: {item_codes}') 
    print(f'type: {type(item_codes)}')

if __name__ == '__main__':
  fire.Fire(Run)
```

```bash
python run1.py 
# => ヘルプが表示される

python run1.py job1
# => ITEM_CODES が必要というエラーになる

python run1.py job1 --item_codes=
# => item_codes:
# => type: <class 'str'>

python run1.py job1 --item_codes=aaa
# => item_codes: aaa
# => type: <class 'str'>

python run1.py job1 --item_codes=aaa,bbb
# => item_codes: ('aaa', 'bbb')
# => type: <class 'tuple'>
```

カンマ区切りの文字列を与えるとタプルになることがわかった
