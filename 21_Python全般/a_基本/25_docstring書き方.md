# docstring の書き方

作成日 2021/07/28

## 参考記事を読む

参考記事 => [\[Python\]可読性を上げるための、docstring の書き方を学ぶ（NumPy スタイル） \- Qiita](https://qiita.com/simonritchie/items/49e0813508cad4876b5a)

> この記事では、NumPy スタイルで話を進めます。

```python
def get_fruit_price(fruit_id, location_id):
    """果物の値段を取得する。

    Parameters
    ----------
    fruit_id : int
        対象の果物のマスタID。
    location_id : int
        対象地域のマスタID。

    Returns
    -------
    fruit_price : int
        対象の果物の値段。
    """
    # ...関数内容省略。
    return fruit_price
```

## NumPy スタイルを学ぶ

[Style guide — numpydoc v1\.2\.dev0 Manual](https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard)
