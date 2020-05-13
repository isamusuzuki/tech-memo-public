# XML データを扱う

作成日 2019/11/20

## 01. xml.etree.ElementTree とは

Python の標準ライブラリのひとつ

ドキュメント => [https://docs.python.org/ja/3/library/xml.etree.elementtree.html](https://docs.python.org/ja/3/library/xml.etree.elementtree.html)

```python
import xml.etree.ElementTree as ET

body = (
    '<?xml version="1.0" encoding="UTF-8" ?>'
    '<ResultSet totalResultsAvailable="1" '
    'totalResultsReturned="1" ok="1" ng="0">'
    '    <Result>'
    '        <Status>OK</Status>'
    '    </Result>'
    '</ResultSet>'
)

# fromstringメソッドでXMLデータを読むと、ルート要素が返る
result_set = ET.fromstring(body)
print(type(result_set))  # <class 'xml.etree.ElementTree.Element'>
print(result_set.tag)  # ResultSet

# XPath構文におけるピリオドは、現在のノードを意味する
status = result_set.find('./Result/Status')
print(type(status))  # <class 'xml.etree.ElementTree.Element'>
print(status.tag)  # Status
print(status.text)  # OK

# findしても見つからなかったときは、Noneが返る
status_ng = result_set.find('./Result2/Status2')
print(type(status_ng))  # <class 'NoneType'>
```

## 02. xml.etree.ElementTree の基本的な使い方

### XML データを読み込む

- `ET.parse(filepath)` ... XML ファイルを開いて中身を読む
- `ET.fromstring(string)` ... XML 形式の文字列を読む

```python
tree = ET.parse('../temp/products1.txt')
result_set = tree.getroot()

result_set = ET.fromstring(body)
```

### XML 要素を検索する

- `Element.find(match)` ... タグ名または XPath で、最初の子要素を検索する
- `Element.findall(match)` ... タグ名または XPath で、直接の子要素のみを検索する

```python
root.findall('./country/neighbor')
# => neighbor孫要素すべて

root.findall('./year/..[@name="Singapore"]')
# => 親要素のname属性がシンガポールになっているyear要素
```

### XML 要素の中身にアクセスする

- `Element.tag` ... 要素名を返す
- `Element.text` ... 要素のテキストコンテンツを返す
- `Element.attrib` ... 属性要素を辞書にして返す
- `Element[n]` ... n 番目の子要素を返す

for ループを使った子要素へのアクセス

```python
import xml.etree.ElementTree as ET

tree = ET.parse('../temp/products.xml')
root = tree.getroot()
summary = root[0][1]
number_of_offers = summary[1]
fba_count = 0
mkt_count = 0
for count in number_of_offers:
    if count.attrib['condition'] in ('new', 'New', 'NEW'):
        if count.attrib['fulfillmentChannel'] == 'Amazon':
            fba_count = int(count.text)
        if count.attrib['fulfillmentChannel'] == 'Merchant':
            mkt_count = int(count.text)
print(f'fba_count => {fba_count}')
print(f'mkt_count => {mkt_count}')
```

## 03. xml.etree.ElementTree がサポートしている XPath 構文

- `./tag`
- `.//tag`
- `[@attrib='value']`
- `[n]`

`contains()`や、`and`、`or`はないので、要注意

ドキュメント => [https://docs.python.org/ja/3/library/xml.etree.elementtree.html#supported-xpath-syntax](https://docs.python.org/ja/3/library/xml.etree.elementtree.html#supported-xpath-syntax)
