# JSON データを取り扱う

作成日 2019/11/01、更新日 2021/11/15

## 01. jsonモジュールを使う

- `json.loads(s)` ... JSON 文字列を辞書データに変換する
- `json.dumps(obj)` ... 辞書データを JSON 文字列に変換する

```python
import json

print(json.dumps({'名前': '鈴木'}))
# => {"\u540d\u524d": "\u9234\u6728"}

print(json.dumps({'名前': '鈴木'}, ensure_ascii=False))
# => {"名前": "鈴木"}

print(json.dumps({'名前': '鈴木'}, ensure_ascii=False, indent=4))
# => {
# =>    "名前": "鈴木"
# => }

print(json.dumps({'6': 7, '4': 5}, sort_keys=True, indent=4))
# => {
# =>    "4": 5,
# =>    "6": 7
# => }
```

## 02. JSON ファイルを作成する

```python
import json

sample_dict = {'message': 'Hello, World!'}
with open('temp/sample.json', mode='w', encoding='utf-8') as f:
    f.write(json.dumps(sample_dict, ensure_ascii=False, indent=4))
```

## 03. JSON ファイルを読む

```python
import json

with open('temp/sample.json', mode='r', encoding='utf-8') as f:
    sample_dict = json.loads(f.read())

print(sample_dict)
# => {'message': 'Hello, World!'}

print(sample_dict['message'])
# => Hello, World!
```
