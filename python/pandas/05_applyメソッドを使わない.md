# apply メソッドを使わずに、あえて 1 行づつ作業する

作成日 2020/05/22

1 行づつデータを取り込んで、API に問い合わせをし、\
その結果を新しい列に取り込みたい\
その API にはスロットルがあるので、1 回づつ待ち時間を入れたい\
あえて行数だけ繰り返すコードを書く

ポイント

- `df.shape[0]` ... 行数を知る
- `df.columns.get_loc(a)` ... 列名から列番号を取得する
- `df.iat[i,j]` ... 行番号と列番号でカラムを指定する

```python
import pandas as pd

# Excelファイルを読む
df = pd.read_excel(INPUT_FILE)

# 行数を知る
row_count = df.shape[0]
print(f'オリジナルの行数 => {row_count}')

# 新しい列を作成する
df['arinashi'] = ''

# 列番号を取得する
jan_column_number = df.columns.get_loc('jan1_code')
new_column_number = df.columns.get_loc('arinashi')

# 行数だけ繰り返す
for i in range(0, row_count):
    # その行のJANコードを知る
    jan = df.iat[i, jan_column_number]
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
