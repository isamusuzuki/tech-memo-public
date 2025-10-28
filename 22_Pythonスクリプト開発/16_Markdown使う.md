# Markdownを使う

作成日 2020/10/28

## 1. Markdownとは？

テキストをHTMLに変換するツール

PyPIサイト => [Markdown - PyPI](https://pypi.org/project/Markdown/)

インストール => `uv add markdown`

サンプルコード

```python
import markdown

with open(md_filename, mode="r", encoding="utf-8") as f:
            md_content = f.read()

html = markdown.markdown(md_content, extensions=["tables"])
```

## 2. 公式ドキュメント（英語）を読む

[Python-Markdown — Python-Markdown 3.9 documentation](https://python-markdown.github.io/)

[Sitemap — Python-Markdown 3.9 documentation](https://python-markdown.github.io/sitemap.html)

[Extensions — Python-Markdown 3.9 documentation](https://python-markdown.github.io/extensions/)

[Tables — Python-Markdown 3.9 documentation](https://python-markdown.github.io/extensions/tables/)
