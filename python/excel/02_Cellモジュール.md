# Cell モジュール

作成日 2019/11/28、更新日 2022/12/02

## 01. Cell モジュールとは

[openpyxl\.cell\.cell module — openpyxl 3\.0\.1 documentation](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.cell.cell.html)

- cell.row ... セルの行番号を返す
- cell.column ... セルの列番号を返す
- cell.value ... セルに値を設定する、もしくは値を得る
- cell.hyperlink ... セルにハイパーリンクを設定する、もしくは値を得る

## 02. styles パッケージ

[openpyxl\.styles package — openpyxl 3\.0\.1 documentation](https://openpyxl.readthedocs.io/en/stable/api/openpyxl.styles.html)

以下の Cell モジュールのプロパティには、特定の syltes パッケージを設定する

- cell.alignment
- cell.border
- cell.fill
- cell.font
- cell.number_format
- cell.style

```python
from openpyxl.styles import Font, PatternFill
from openpyxl.styles.alignment import Alignment

ws.cell(row=2, column=4).value = 5678
ws.cell(row=2, column=4).number_format = '#,##0'
ws.cell(row=2, column=4).alignment = \
    Alignment(horizontal='center')
ws.cell(row=2, column=4).font = \
    Font(underline='single', color='FF0563c1')
ws.cell(row=2, column=4).fill = \
    PatternFill(patternType='solid', fgColor='d3d3d3')

# リンクを入れるときは、自分で青色と下線を追加する必要ある
ws.cell(row=j, column=10).value = rankingTitle
ws.cell(row=j, column=10).hyperlink = rankingUrl
ws.cell(row=j, column=10).font = \
    Font(underline='single', color='FF0563c1')
```

## 03. 計算式を入力する

cell の指定に番号を使ったとしても、計算式の中でのセルの指定は、A1 形式を使う

```python
ws.cell(row=current, column=15).value = \
    f'=IF(M{current} < F{current}, TRUE, FALSE)'
```

## 04. 日付を入力する

```python
from datetime import date

ws.cell(row=1, column=15).value = date(2022, 12, 2)
```
