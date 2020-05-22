# time を使う

作成日 2020/03/26

## 経過時間を記録する

```python
from time import time

print('作業を開始します。しばらくお待ちください')
t1 = time()

for i in range(1000000):
    i ** 10

t2 = time()
elapsed_time = t2 - t1
print (f'作業が終わりました。{elapsed_time:0.2f}秒かかりました')
```

## スクリプトに「待たせる」

sleep 関数の引数は、秒数

```python
from time import sleep

# 3秒待たせる
sleep(3)
```
