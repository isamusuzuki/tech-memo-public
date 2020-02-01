# Pandas でファイル読み書き

作成日 2019/12/23

## 01. CSV ファイルを読み込む

csv ファイルは、`pd.read_csv()` メソッドでを読むと、\
DataFrame オブジェクトとして取り扱えるようになる

TSV（タブ区切り）ファイルは、`pd.read_table()`メソッドで読む

```python
import pandas as pd

# タブ区切りのファイルを読み込み、DataFrameという行列型に変換する
df = pd.read_table('./temp/10-09-2019.txt')

# 行列のサイズを知る
print(df.shape)
# => (23912, 34)

# 条件を指定して絞り込み、1列だけ取り出す
a = df[df['ASIN 1'] == 'B01ABCXXXX']['出品者SKU']
# => 戻り値はSeriesになる

print(a.values[0])
# => hot1234-potato-chip
```

ファイルが見つからなかったときは、`FileNotFoundError`が投げられる

```python
import pandas as pd

def apple(file_path):
    try:
        df = pd.read_table(file_path)
        return {'success': True, data: df}
    except FileNotFoundError as err:
        return {'success': False, message: err}
```

## 02. CSV ファイルを生成する

- シフト JIS に変換すると、機種依存文字（ソフト JIS に対応がないユニコード文字）が変換できずにエラーになる
- Unicode(UTF-8)で作成した CSV ファイルは、Excel で開くと文字化けを起こす
- BOM 付きの Unicode で CSV ファイルを作成すれば、エラーも起こさず、Excel で開いても文字化けを起こさない

```python
# シフトJISでCSVファイルを生成する
df.to_csv('data.csv', encoding='cp932', sep=',' ,index=False)

# BOM付きのUnicodeでCSVファイルを生成する
df.to_csv('data.csv', encoding='utf-8-sig', sep=',' ,index=False)
```

## 03. Excel ファイルを読み込む

xlrd が必要 => `pip install xlrd`

```python
import pandas as pd

df = pd.read_excel('./data/exel.xlsx')

print(df.shape)
```

## 04. Excel ファイルを生成する

openpyxl が必要 => `pip install openpyxl`

```python
df.to_excel('data.xlsx', sheet_name='new_sheet_name')
```
