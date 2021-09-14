# JSON ファイル

作成日 2021/03/05

## 01. JSON ファイルを作成する

```python
import json

sample_dict = {'message': 'Hello, World!'}
with open('temp/sample.json', mode='w', encoding='utf-8') as f:
    f.write(json.dumps(sample_dict, ensure_ascii=False, indent=4))
```

## 02. JSON ファイルを読む

```python
import json

with open('temp/sample.json', mode='r', encoding='utf-8') as f:
    sample_dict = json.loads(f.read())

print(sample_dict['message'])
# => Hello, World!
```
