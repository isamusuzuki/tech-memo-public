# apply メソッドを使わずに、あえて 1 行づつ作業する

作成日 2020/05/22、更新日 2025/12/17

## 01. 課題

- 1 行づつJANコードを読み込んで、API に問い合わせをし、その結果を新しい列に取り込みたい
- その API にはスロットルがあるので、1 回づつ待ち時間を入れたい

=> あえて行数分、繰り返すコードを書く

## 02. サンプルコード

- `df.shape[0]` ... 行数を知る
- ~~`df.columns.get_loc(a)` ... 列名から列番号を取得する~~
- `columns_list = df.columns.tolist()` ... 列名のリストを取得する
- `columns_list.index(a)` ... 列名のリストの何番目にあるかを知る
- `df.iat[i,j]` ... 行番号と列番号でカラムを指定する（戻り型はScalar）
- `str(df.iat[i,j])` ... 型としてのScalarは使いにくいので文字列に変換
- `int(str(df.iat[i,j]))` ... 整数にしたい場合は、いったん文字列に変換する

```python
import pandas as pd

# Excelファイルを読む
df = pd.read_excel(excel_file))

# 行数を知る
row_count = df.shape[0]
print(f'オリジナルの行数 => {row_count}')

# 新しい列を作成する
df['arinashi'] = ''

# 列番号を取得する
columns_list = df.columns.tolist()
jan_column_number = columns_list.index('jan1_code')
new_column_number = columns_list.index('arinashi')

# 行数だけ繰り返す
for i in range(0, row_count):
    # その行のJANコードを知る
    jan = str(df.iat[i, jan_column_number])
    # APIに問い合わせする
    response = m.get(jan)
    # 新しい列に結果を入力する
    df.iat[i, new_column_number] = \
        'ARI' if response['success'] else 'NASHI'
    # スロットルを超過しないように待つ
    sleep(0.2)

# ありのものだけを取得する
df1 = df[df['arinashi'] == 'ARI']
print(f'「あり」だけの行数 => {df1.shape[0]}')

# なしのものだけを取得する
df2 = df[df['arinashi'] == 'NASHI']
print(f'「なし」だけの行数 => {df2.shape[0]}')
```
