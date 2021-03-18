# dtype

作成日 2020/10/15、更新日 2021/02/28

## 01. dtype とは

[pandas のデータ型 dtype 一覧と astype による変換（キャスト） \| note\.nkmk\.me](https://note.nkmk.me/python-pandas-dtype-astype/)

> pandas.Series は一つのデータ型 dtype を、
> panas.DataFrame は各列ごとにそれぞれデータ型 dtype を保持している。
>
> dtype は、コンストラクタで新たにオブジェクトを生成する際や csv ファイルなどから
> 読み込む際に指定したり、astype()メソッドで変換（キャスト）したりすることができる。

### dtype 一覧

- 整数型      ... int32,int64,uint32,uint64
- 浮動小数点型 ... float32,float64
- 複素数型    ... complex64
- ブール型    ... bool
- Python オブジェクト型 ... object

文字列型はない。日付型もない

## 02. データ型を変更する

```python
import pandas as pd

df = pd.read_csv('temp/data.csv', encoding='cp932', dtype=object)

df["年齢float"] = df["年齢"].astype(float)
```

### エラーに対処する

`ValueError: Cannot convert non-finite values (NA or inf) to integer`というエラーに直面した

[Python: pandas でカラムの型を変換する \- CUBE SUGAR CONTAINER](https://blog.amedama.jp/entry/2017/11/01/211110)

> NaN が入っている場合に、Series#astype() メソッドを使うと ValueError 例外になってしまう。
>
> そういったときは Series#fillna() メソッドなどを使って NaN をそれ以外の値に置き換えてやる必要がある。 もちろん Series#dropna() などで、そもそも集計の対象外としてしまうことも考えられる。

```python
# エラーになった
df['supplier_code'] = df['supplier_code'].astype(int)

# 対処した
df['supplier_code'] = df['supplier_code'].fillna(0).astype(int)
```
