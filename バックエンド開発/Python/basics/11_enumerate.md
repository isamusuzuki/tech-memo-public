# enumerate をもっと使おう

作成日 2020/08/28

## enumerate とは

組み込み関数のひとつ。enumerate オブジェクトを返す。\
中身は、カウントとイテレーションで得られた値のタプルで\
カウントは第 2 引数で開始番号を指定できる

range を使うよりもコードがシンプルになることは間違いない

構文 => `enumerate(iterable, start=0)`

[組み込み関数 — Python 3\.8\.5 ドキュメント](https://docs.python.org/ja/3/library/functions.html)

## 使用例

```python
seasons = ['春', '夏', '秋', '冬']

i = 1
for s in seasons:
    print(f'{i:03}: {s}')
    i += 1

for i in range(len(seasons)):
    print(f'{i+1:03}: {seasons[i]}')

for i, s in enumerate(seasons, start=1):
    print(f'{i:03}: {s}')

# => 001: 春
# => 002: 夏
# => 003: 秋
# => 004: 冬
```
