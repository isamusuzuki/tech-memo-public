# gzip ファイルを作成する

作成日 2020/11/09

## gzip でファイルを圧縮する

サンプルコード

```python
import gzip
import shutil

json_file = 'temp/data.json'
gz_file = f'data.json.gz'
local_gz_file = f'temp/{gz_file}'
remote_gz_file = f'upload/{gz_file}'

with open(json_file, 'rb') as f_in:
    with gzip.open(local_gz_file, 'wb') as f_out:
        shutil.copyfileobj(f_in, f_out)
```
