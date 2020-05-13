# ローカルで実行する Pyhon スクリプトを書く

作成日 2020/02/03、更新日 2020/03/26

## 01. 直接実行したときだけスクリプトが動くようにする

`apple.py` というファイルを作成し、`python apple.py` を実行する

```python
print (__name__)
# => __main__

print (__file__)
# => apple.py
```

- 結果が `__main__` になったのは、`apple.py` を直接実行したから
- 別ファイルから `import apple` していたら、結果は `apple` になる

この`__name__`変数を利用して、直接実行したときだけ動くコードを書く

```python
def apple(name):
    print(f'hello {name}!')


if __name__ == "__main__":
    apple('world')
```

## 02. 引数を使う

```python
import sys

try:
    report_type = sys.argv[1]
except IndexError:
    print('Please input report_type')
    exit()

pass # => report_typeを使う
```

## 03. データをローカルディスクに保存する

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

## 04. スクリプトに「待たせる」

sleep 関数の引数は、秒数

```python
from time import sleep

# 60秒待たせる
sleep(60)
```

## 05. 外部コマンドを実行する

### subprocess.call()

`call()`メソッドを使うと、終了待ちをする。\
Python のプロセスは、ブロックされる

```python
import subprocess

cmd = 'sleep 30'
proc = subprocess.call(cmd, shell=True)
```

### subprocess.Popen()

`Popen()`メソッドを使うと、終了待ちをせずに、\
サブプロセスを起動したら、Python のプロセスは、先に進む。\
親プロセスの Python が、サブプロセスよりも先に終了してしまったら、\
孤児プロセスとして、root プロセスに引き取られる

```python
proc = subprocess.Popen(cmd, shell=True)
```

#### Popen()メソッドでも、終了待ちさせることは可能

```python
proc = subprocess.Popen(cmd,shell=True)
proc.wait()
```

#### Popen()メソッドなら、できること

コンソールにプロセス ID を表示する

```python
proc = subprocess.Popen(cmd, shell=True)
print(f'process {proc.pid} starts')
proc.wait()
```

### subprocess.getoutput()

サブプロセスの戻り値を取得できる

```python
from subprocess import getoutput

for color in spec['colors']:
    cmd = f'identify -format "%w,%h" temp/{color["filename"]}'
    size = getoutput(cmd).split(',')
    width = int(size[0])
    height = int(size[1])
```

## 06. 作業の経過時間を記録する

```python
import time

print('作業を開始します。しばらくお待ちください')
t1 = time.time()

for i in range(1000000):
    i ** 10

t2 = time.time()
elapsed_time = t2 - t1
print (f'作業が終わりました。{elapsed_time:0.2f}秒かかりました')
```

## 07. ネットワークドライブにローカルドライブを割り当てる

NET USE コマンドは、コマンドプロンプト、Powershell、Git Bash\
いずれのシェルからでも使用可能

```bash
# ネットワークドライブの接続を設定する
NET USE X: \\192.168.2.100\公開フォルダ

# ネットワークドライブの一覧を表示する
NET USE

# ネットワークドライブの接続を解除する
NET USE Z: /delete
```

conn.bat

```bash
NET USE X: \\192.168.2.100\公開フォルダ
```

disconn.bat

```bash
NET USE X: /delete
```

main.py

```python
import subprocess

cmd = 'conn.bat'
proc = subprocess.Popen(cmd, shell=True)
print(f'process {proc.pid} starts: ネットワークドライブに接続しています')
proc.wait()

pass # => X:ドライブを使う

cmd = 'disconn.bat'
proc = subprocess.Popen(cmd, shell=True)
print(f'process {proc.pid} starts: ネットワークドライブを切断しています')
proc.wait()
```

**要注意** バッチファイルは、必ずシフト JIS で保存すること
