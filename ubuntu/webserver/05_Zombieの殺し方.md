# Zombieプロセスの殺し方

作成日 2020/09/10

gunicorn を systemdのサービスにして、pythonコードで、\
subprocessを呼んだ場合、スクリプトがエラーを出すと、Zombieプロセスになってしまう

```python
import subprocess

cmd = f'./script.sh sample.py{check_param()}'
proc = subprocess.Popen(cmd, shell=True, executable='/bin/bash')
```

## Zombieプロセスを見つける

```bash
ps aux | grep Z

kill {PID}
# => 本来ならば、これで削除できるはずだが、
# => 親プロセスがあるために死なない

sudo systemctl stop bobby
# => 親プロセスをいったん止めることで
# => はじめて子プロセスが止まる
```
