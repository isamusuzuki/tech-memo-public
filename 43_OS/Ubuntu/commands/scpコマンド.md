# scp コマンド

作成日 2020/09/01

## 01. scp コマンドの基本

構文: `scp オプション ログイン名@ホスト名:パス ログイン名@ホスト名:パス`

オプション

- `-i` ... SSH 秘密鍵を指定する
- `-r` ... ローカルのディレクトリごとリモートにコピーする

使用例

```bash
# 最も簡単な例
scp ~/test.txt ubuntu@192.168.3.251:~/test.txt

# コピー先のファイル名は省略可能
scp ~/test.txt ubuntu@192.168.3.251:~

# SSH秘密鍵を使う
scp -i ~/.ssh/awsweb.pem ~/test.txt ec2-user@100.100.100.100:~

# フォルダごとまとめてコピーする
scp -r ./ centos@10.0.0.224:/opt/bitnami/apps/wordpress/htdocs/work/
```
