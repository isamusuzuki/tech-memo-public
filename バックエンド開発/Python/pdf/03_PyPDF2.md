# PDF ファイルを暗号化するために PyPDF2 を使う

作成日 2020/05/13

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
