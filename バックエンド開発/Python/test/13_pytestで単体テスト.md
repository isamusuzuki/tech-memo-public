# pytest で単体テストを行う

作成日 2022/08/30

## pytest とは

サードパーティー製ながら、使い勝手のいい、単体テスト用のフレームワーク

[pytest · PyPI](https://pypi.org/project/pytest/)

pytest のインストール => `pip install pytest`

公式ドキュメント => [https://docs.pytest.org/](https://docs.pytest.org/)

## 自分なりの理解

- unittest は標準モジュールである一方で、pytest は別途インストールする必要がある
- unittest は`assert`から始まるメソッドが多数ある一方で、pytest は`assert`文しか使わない
- unittest が`unittest.TestCase`を継承するクラスをつくる一方で、pytest はメソッドをつくるだけ
- pytest コマンドを実行すると、自動で`test_`から始まるメソッドを探して、まとめて実行する

## サンプルコード

ファイル・フォルダ構造

```text
--avocado/
    |--apps/
    |   `--calc/
    |       `--haiso.py
    `--tests/
        `--test_haiso.py
```

test_haiso.py

```python
from apps.calc.haiso import HaisoKeisan


def test_1():
    hk = HaisoKeisan()
    hd = hk.calc(
        hasso_tate=22.0, hasso_yoko=11.0,
        hasso_atsusa=8.0, total_weight=211.0
    )
    assert hd.haiso_method == '250定形外'
    assert hd.haiso_cost == 319.0
```

スクリプト実行

```bash
pytest
```

※ pytestは、unittestのコードも探し出して、実行する。書き換えることなく移行が可能

### 気を付けるべきこと

- `tests` フォルダに、`__init__()` メソッドを持つクラスを置いても、pytestは実行できない
