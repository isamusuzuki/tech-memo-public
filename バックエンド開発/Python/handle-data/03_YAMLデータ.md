# YAML データを取り扱う

作成日 2019/11/01

```python
import yaml

data = {
    'item_code': 'atc1222',
    'style': 'ladies',
    'color_start_number': 8,
    'gousei_pattern': 'foursome',
    'colors': [
        {'filename': 'c1.jpg', 'colorname': 'BK.ブラック'},
        {'filename': 'c2.jpg', 'colorname': 'BL.ブルー'},
        {'filename': 'c3.jpg', 'colorname': 'GR.グリーン'},
        {'filename': 'c4.jpg', 'colorname': 'GY.グレー'},
        {'filename': 'c5.jpg', 'colorname': 'NV.ネイビー'}
    ]
}

with open('temp/test.yml', mode='w', encoding='utf-8') as f:
    yaml.dump(data, f, encoding='utf-8', allow_unicode=True)
```

test.yml

```yaml
color_start_number: 8
colors:
    - colorname: BK.ブラック
      filename: c1.jpg
    - colorname: BL.ブルー
      filename: c2.jpg
    - colorname: GR.グリーン
      filename: c3.jpg
    - colorname: GY.グレー
      filename: c4.jpg
    - colorname: NV.ネイビー
      filename: c5.jpg
gousei_pattern: foursome
item_code: atc1222
style: ladies
```
