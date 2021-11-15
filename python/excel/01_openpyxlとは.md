# openpyxl

作成日 2019/11/28、更新日 2021/09/28

## 01. openpyxl とは

ドキュメント => [https://openpyxl.readthedocs.io/en/stable/](https://openpyxl.readthedocs.io/en/stable/)

インストール => `pip install openpyxl`

最新バージョン => 3.0.9 (2021/09/23)

## 02. 既存の Excel ファイルを読む

```python
from openpyxl import load_workbook

# 読み取り専用でエクセルファイルを開く
wb = load_workbook(filename=file_path, read_only=True)
# 決め打ちで1枚目のシートを読む
ws = wb.worksheets[0]

# セルの場所も決め打ちで読む
result.data['namae_onchu'] = ws['B2'].value
result.data['order_no'] = ws['C6'].value

# ファイル名を指定して別ファイルに保存可能
wb.save(EXCEL_FILE)
```

## 03. Excel ファイルを作成する

### もっとも簡単な例

```python
import openpyxl as excel

wb = excel.Workbook()
ws = wb.active

ws['A1'] = 'こんにちは'
wb.save(EXCEL_FILE)
```

### cell()メソッドを使った簡単な例

```python
import openpyxl as excel

wb = excel.Workbook()
ws = wb.active
for i in range(1, 10):
    for j in range(1, 10):
        v = i * j
        ws.cell(column=j, row=i, value=v)
wb.save(EXCEL_FILE)
```

### cell()メソッドを使った応用例

```python
import openpyxl as excel
from openpyxl.styles.alignment import Alignment

wb = excel.Workbook()
ws = wb.active

# 列幅を設定する
ws.column_dimensions['A'].width = 5
ws.column_dimensions['B'].width = 15
ws.column_dimensions['C'].width = 60
ws.column_dimensions['D'].width = 10

# cellを使って値とスタイルを入力していく
# タイトル行
ws.cell(row=1, column=1).value = '#'
ws.cell(row=1, column=1).alignment = Alignment(horizontal='center')
ws.cell(row=1, column=2).value = 'ASIN'
ws.cell(row=1, column=3).value = 'タイトル'
ws.cell(row=1, column=4).value = '実価格'
ws.cell(row=1, column=4).alignment = Alignment(horizontal='right')
# データ行
ws.cell(row=2, column=1).value = 1
ws.cell(row=2, column=1).alignment = Alignment(horizontal='center')
ws.cell(row=2, column=2).value = 'ABCDEFGH'
ws.cell(row=2, column=3).value = 'タイトルタイトル'
ws.cell(row=2, column=4).value = 5678
ws.cell(row=2, column=4).number_format = '#,##0'

wb.save(EXCEL_FILE)
```
