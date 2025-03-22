# HTML データを取り扱う

作成日 2021/03/24

## 01. lxml とは

lxml は XML と HTML を処理する Python ライブラリ

その中で `lxml.html` が HTML データに特化している

公式 => [https://lxml.de/lxmlhtml.html](https://lxml.de/lxmlhtml.html)

### lxml.html の基本的な使い方

- `html.parse(filename_url_or_file)` ... `read()`メソッドをもったものを読む
- `html.fromstring(string)` ... 全体か一部分かにかかわらず、文字列を読む
- `html.tostring(element)` ... バイト文字列を出力する

## 02. サンプルコード

### 商品説明テーブルから、タイトルが「素材」の中身だけを抜き出す

```python
from lxml import html

def helper(obj):
    content = html.fromstring(obj)
    titles = content.xpath('//td[@class="shohin-detail-title"]')
    nakamis = content.xpath('//td[@class="shohin-detail-nakami"]')
    materials = '記述なし'
    for i in range(0, len(titles)):
        if titles[i].text == '素材':
            if len(nakamis) > i:
                materials = nakamis[i].text
                break
    return materials
```

### 商品説明 HTML から、テーブル要素以外を除外する

```python
from lxml import html

def helper(obj):
    content = html.fromstring(obj)
    tables = content.xpath('//table')
    return html.tostring(tables[0], encoding='utf-8').decode('utf-8')
```
