# ReportLab を使う

作成日 2020/05/13

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

## 日本語を使う

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
