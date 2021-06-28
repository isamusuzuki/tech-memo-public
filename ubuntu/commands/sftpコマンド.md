# sftp コマンド

作成日 2021/06/28

## 01. sftp コマンドの基本

構文: `scp オプション ログイン名@ホスト名`

オプション

- `-i` ... SSH 秘密鍵を指定する

### 対話的なコマンド一覧

- `bye` ... 接続を閉じる
- `cd <path>` ... 指定したパスに移動
- `get <file>` ... ファイルをダウンロードする
- `put <file>` ... ファイルをアップロードする
- `pwd` ... リモート側の現在ディレクトリ

## 02. sftp コマンドの使用例

```bash
which sftp
# => /usr/bin/sftp

# 接続する
sftp sftpuser@100.100.100.100
# sftpuser@100.100.100.100's password:
# Connected to 100.100.100.100.
# sftp>
```
