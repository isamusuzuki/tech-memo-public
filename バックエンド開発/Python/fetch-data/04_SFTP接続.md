# Paramiko を使って SFTP サーバーに接続する

作成日 2020/11/09

## 1. Paramiko とは

[Welcome to Paramiko\! — Paramiko documentation](https://www.paramiko.org/)

> Paramiko is a Python (2.7, 3.4+) implementation of the SSHv2 protocol, providing both client and server functionality.

ドキュメント => [Welcome to Paramiko’s documentation\! — Paramiko documentation](http://docs.paramiko.org/en/stable/)

## 2. サンプルコード

### 2-1. SFTP サーバーにファイルをアップロードする

```python
import paramiko

# SFTPサーバーに接続する
client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy)
client.connect(
    HOSTNAME, port=21,
    username=USERNAME, password=PASSWORD)
try:
    # SFTPセッションを開始する
    sftp_connection = client.open_sftp()
    sftp_connection.put(local_gz_file, remote_gz_file)
    response = {
        'success': True,
        'message': gz_file
    }
except Exception as err:
    response = {
        'success': False,
        'message': f'ERROR => {err}'
    }
finally:
    client.close()
    return response
```

### 2-2. SFTP サーバーのディレクトリ上のファイル一覧を取得する

```python
import paramiko


# SFTPサーバーに接続する
client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy)
client.connect(
    HOSTNAME, port=21,
    username=USERNAME, password=PASSWORD)

# SFTPセッションを開始する
sftp_connection = client.open_sftp()

# ディレクトリを変更する
sftp_connection.chdir('upload')

# ディレクトリの中身を取得する
paths = sftp_connection.listdir()
for i, s in enumerate(paths):
    print(f'{i + 1}: {s}')

# サーバーへの接続を切断する
sftp_connection.close()
```
