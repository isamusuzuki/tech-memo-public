# Pandas を使いこなす

作成日 2019/12/23

## 01. DataFrame 上で加工する

### 条件で絞り込み、取り出す列を指定する

```python
a = df[(df['state'] == 'Ohio') & (df['year'] == 2002)]['pop']
# => 戻値は、Seriesになる

print(a)
# => 2    3.6
# => Name: pop, dtype: float64

print(a.values[0])
# => 3.6
```

取り出したい列が複数になる場合は、二重カッコで囲む

```python
a = df[df['state'] == 'Ohio'][['year', 'pop']]`
```

### 複数条件で絞り込み、取り出す列を指定する

```python
a = df[(df['state'] == 'Ohio') & (df['year'] == 2002)]['pop']
```

真偽が入っている 3 つの列のすべてが真のものだけを抽出する

```python
import pandas as pd

a0 = pd.read_excel('./data/20191017.xlsx')
a1 = a0[['sku_code', '赤字確認', '価格確認', 'レビュー確認']]
a2 = a1[a1['赤字確認'] & a1['価格確認'] & a1['レビュー確認']]

print(a2.shape)
```

### データ型を変更する

```python
# オリジナルのオブジェクトを変更しない
a["年齢"].astype(float)

# 新しい値で元のオブジェクトを上書きする
a["年齢"] = a["年齢"].astype(float)
```

### 列名を変更する

```python
df2 = df1.rename(columns={'出品者SKU': 'sku_code'})
```

### DataFrame の 1 列分をリストに変換する

Series に変換してから、values プロパティをリストに変換する

```python
df = pd.read_excel('./data.xlsx)
s = df[df['数量' > 0]['商品コード']
#=> 戻り値はSeriesになっている

item_list = s.values.tolist()
total_count = len(item_list)
```

## 02. dtype を学ぶ

[pandas のデータ型 dtype 一覧と astype による変換（キャスト） \| note\.nkmk\.me](https://note.nkmk.me/python-pandas-dtype-astype/)

> pandas.Series は一つのデータ型 dtype、\
> panas.DataFrame は各列ごとにそれぞれデータ型 dtype を保持している。
>
> dtype は、コンストラクタで新たにオブジェクトを生成する際や csv ファイルなどから\
> 読み込む際に指定したり、astype()メソッドで変換（キャスト）したりすることができる。

### dtype 一覧

- 整数型 ... int32,int64,uint32,uint64
- 浮動小数点型 ... float32,float64
- 複素数型 ... complex64
- ブール型 ... bool
- Python オブジェクト型 ... object

日付型はない。文字列型もない

### 日付型を導入する

[Pandas で時間や日付データに変換する to_datetime 関数の使い方 \- DeepAge](https://deepage.net/features/pandas-to-datetime.html)

> Pandas において文字列データや数値データを日付データである datetime64 型に変換する
>
> datetime64 型は Python にある timestamp 型を継承したクラス

```python
df['order_at'] = pd.to_datetime(df['order_at'])
```

## 03. エラーに対処する

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
