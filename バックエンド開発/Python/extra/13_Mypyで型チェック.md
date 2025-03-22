# mypyを使って型チェックする

作成日 2020/07/19

## Mypyとは

Pythonの静的型チェッカー

公式トップ => [mypy \- Optional Static Typing for Python](http://mypy-lang.org/)

ドキュメント => [Welcome to Mypy documentation\!](https://mypy.readthedocs.io/en/stable/)

## アノテーションとは

Python 3.6 で、アノテーションが採用された

チートシート => [Type hints cheat sheet \(Python 3\)](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html)

## typing.Protocolとは

[PythonでProtocolを使って静的ダック・タイピング \- Qiita](https://qiita.com/spicy_laichi/items/29ef79eac29d61fcb503)

```python
from typing import Protocol

class Animal(Protocol):
    def sound(self) -> str:


class Dog()
    def sound(self) -> str:
        return "Bow wow"

def func(animal: Animal):
    animal.sound()
```
