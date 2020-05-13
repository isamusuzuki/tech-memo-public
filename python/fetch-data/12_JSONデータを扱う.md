# JSON データを取り扱う

作成日 2019/11/01

-   `json.loads(s)` ... JSON 文字列を辞書データに変換する
-   `json.load(fp)` ... `read()`メソッドを持つオブジェクトからデータを読んで辞書データに変換する
-   `json.dumps(obj)` ... 辞書データを JSON 文字列に変換する
-   `json.dump(obj, fp)` ... 辞書データをオブジェクトへのストリームとしてシリアライズする

```python
import json

print(json.dumps({'名前': '鈴木'}))
#=> {"\u540d\u524d": "\u9234\u6728"}

print(json.dumps({'名前': '鈴木'}, ensure_ascii=False))
#=> {"名前": "鈴木"}

print(json.dumps({'6': 7, '4': 5}, sort_keys=True, indent=4))
#=> {
#=>    "4": 5,
#=>    "6": 7
#=> }
```
