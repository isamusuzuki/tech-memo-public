# FTP サーバーに接続する

作成日 2020/10/01

## 01. 基本形

FTP サーバーにアクセスしてファイルをアップする

```python
from ftplib import FTP


def ftp_upload(settei):
    # FTPサーバーに接続する
    ftp = FTP()
    ftp.set_pasv("true")
    ftp.connect(
        host=settei['address'],
        port=settei['port']
    )
    ftp.login(
        user=settei['account'],
        passwd=settei['password'],
        acct=settei['account']
    )
    # ディレクトリを移動する
    ftp.cwd(settei['path'])

    # サブディレクトリを確認する
    for d in settei['dirs']:
        ftp_current_dirs = ftp.nlst()
        if d['dir'] not in ftp_current_dirs:
            # サブディレクトリがない場合は作成する
            ftp.mkd(f'{settei["path"]}/{d["dir"]}')

    # ファイルをアップロードする
    for fl in settei['files']:
        fp = open(fl['local_path'] , mode='rb')
        ftp.storbinary(f'STOR {fl["ftp_path"]}', fp)
        fp.close()

    # FTPサーバーへの接続を切断する
    ftp.close()
```

## 02. FTP_TLS接続を行う

```python
from ftplib import FTP_TLS


def ftp_upload(settei):
    # FTPサーバーに接続する
    ftp = FTP_TLS()
    ftp.set_pasv("true")
    ftp.connect(
        host=settei['address'],
        port=settei['port']
    )
    ftp.login(
        user=settei['account'],
        passwd=settei['password'],
        acct=settei['account']
    )
    # データ接続をセキュアにする
    ftp.prot_p()
    # 以下同じ
```
