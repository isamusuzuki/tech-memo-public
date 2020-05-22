# shelf を使う

作成日 2020/03/26

## データを保存する

shelf を使う。shelve は shelf の複数形

```python
import shelve

# データを保存する
shelf = shelve.open(c.SHELF_FILE)
shelf['report_type'] = report_type
shelf.close()

# 保存したデータを読む
shelf = shelve.open(c.SHELF_FILE)
report_type = None
if 'report_type' in shelf:
    report_type = shelf['report_type']
shelf.close()

if report_type:
    pass # => report_typeを使う
else:
    print('Error => no report_type')
    exit()
```
