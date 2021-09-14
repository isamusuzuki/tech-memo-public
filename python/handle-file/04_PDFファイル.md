# PDF ファイル

作成日 2020/05/13

## 01. ReportLab を使う

公式トップ => [ReportLab \- Content to PDF Solutions](https://www.reportlab.com/)

ドキュメント => [Documentation \- ReportLab\.com](https://www.reportlab.com/dev/docs/)

詳細はユーザーガイド PDF の中にあり

インストール　=> `pip install reportlab`

```python
from reportlab.pdfgen.canvas import Canvas
from reportlab.lib.pagesizes import A4, portrait

c = Canvas('helloworld.pdf', pagesize=portrait(A4))
width, height = A4
c.drawCentredString(width/2, height/2, 'Hello, World!')
c.showPage()
c.save()
```

### 日本語を使う

使う前に日本語フォントを登録する必要がある

```python
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont

GEN_SHIN_GOTHIC_MEDIUM_TTF = \
    "./fonts/GenShinGothic-Monospace-Medium.ttf"
pdfmetrics.registerFont(
    TTFont('GenShinGothic', GEN_SHIN_GOTHIC_MEDIUM_TTF))
font_size = 20
c.setFont('GenShinGothic', font_size)

c.drawString(x, y, text)
c.drawRightString(x, y, text)
c.drawCenteredString(x, y, text)
```

参考記事 => [Python で日本語を PDF に出力する（ReportLab を利用） \| ガンマソフト株式会社](https://gammasoft.jp/blog/pdf-japanese-font-by-python/)

## 02. PDF ファイルを暗号化するために PyPDF2 を使う

公式トップ => [Home page for the PyPDF2 project](http://mstamy2.github.io/PyPDF2/)

ドキュメント => [PyPDF2 Documentation — PyPDF2 1\.26\.0 documentation](https://pythonhosted.org/PyPDF2/)

インストール => `pip install PyPDF2`

```python
from PyPDF2 import PdfFileReader, PdfFileWriter

output_pdf = PdfFileWriter()

with open('helloworld.pdf', mode='rb') as f:
    input_pdf = PdfFileReader(f)
    output_pdf.appendPagesFromReader(input_pdf)

output_pdf.encrypt('password')

with open('helloword2.pdf', mode='wb') as f:
    output_pdf.write(f)

print('done')
```
