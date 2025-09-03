# enumerate

作成日 2020/08/28、更新日 2025/07/28

## 1. enumerate とは

組み込み関数のひとつ。enumerateオブジェクトを返す。\
中身は、カウントとイテレーションで得られた値のタプルで\
カウントは第2引数で開始番号を指定できる

## 2. 生成AIが教えてくれたCSVを100行ごとに分割するコード

```python
input_filename = "temp/input.csv"
output_filename_base = "temp/output_"

df = pd.read_csv(input_filename, encoding="utf-8").fillna("")

# 100行ごとに分割してJSONファイルに保存する
chunk_size = 100
for i, start in enumerate(range(0, len(df), chunk_size), 1):
    chunk = df.iloc[start : start + chunk_size]
    output_filename = f"{output_filename_base}{i}.json"
    chunk.to_json(
        output_filename, orient="records", force_ascii=False, indent=2
    )
    logger.info(
        f'"{output_filename}"を保存しました (行: {start}〜{start + len(chunk) - 1})'
    )
```

### 2a. 組み込み関数の確認

公式ドキュメント => [組み込み関数](https://docs.python.org/ja/3/library/functions.html)

`range(start, stop, step)`

```python
list(range(0, 397, 100))
# => [0, 100, 200, 300, 397]
```

`enumerate(iterable, start=0)`

```python
list(enumerate(range(0, 397, 100), 1))
# => [(1, 0), (2, 100), (3, 200), (4, 300), (5, 397)]
```
