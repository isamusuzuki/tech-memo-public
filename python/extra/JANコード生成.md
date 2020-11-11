# JANコードを生成する

作成日 2019/11/29

## 01. Python で JAN コードを生成する

main.py を動かすには、以下の 2 つのパッケージが必要

```bash
pip install python-barcode
pip install Image
```

main.py

```bash
import barcode
from barcode.writer import ImageWriter

ean = barcode.get('ean13', '5901234123457', writer=ImageWriter())

with open('temp/test.png', mode='wb') as f:
    ean.write(f)

print('done')
```

## 02. パッケージの選定について

[pyBarcode · PyPI](https://pypi.org/project/pyBarcode/)

=> Python 3.7 にインストールすることができなかった

`ERROR: Could not find a version that satisfies the requirement pyBarcode (from versions: none)`

[python\-barcode · PyPI](https://pypi.org/project/python-barcode/)

=> Python 3.7 を正式サポートしていた

PNG 画像を生成するためには、Image パッケージをインストールする必要があった
