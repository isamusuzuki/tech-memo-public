# XMLデータを作る

作成日 2021/05/06

## XML要素を組み立てる

```python
import xml.etree.ElementTree as ET

top = ET.Element('top')

comment = ET.Comment('Generated for PyMOTW')
top.append(comment)

child = ET.SubElement(top, 'child')
child.text = 'This child contains text.'

child_with_tail = ET.SubElement(top, 'child_with_tail')
child_with_tail.text = 'This child has regular text.'
child_with_tail.tail = 'And "tail" text.'

child_with_entity_ref = ET.SubElement(top, 'child_with_entity_ref')
child_with_entity_ref.text = 'This & that'

print(ET.tostring(top))
```

## 読みやすくレイアウトされたXMLファイルを生成する

```python
from xml.dom import minidom

def accessapi():
    handler = AccessApi()
    response = handler.folder_get(
        offset=1, limit=30
    )
    if not result['success']
        print(result)
    else:
        pretty_xml = \
            minidom.parseString(result['body']).toprettyxml()
        xml_file = 'temp/test1.xml'
        with open(xml_file, mode='w', encoding='utf-8') as f:
            f.write(pretty_xml)
        print(f'done => {xml_file}')
```
