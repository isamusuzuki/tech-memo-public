# Windows 共有フォルダをマウントする

作成日 2020/12/07

## 01. とりあえず設定してみる

```bash
# パッケージのインストール
sudo apt install cifs-utils

# 準備
sudo mkdir -p /mnt/y
sudo mkdir -p /mnt/z

# マウント
sudo mount -t cifs -o username=nnnnnnnn,password=xxxxxxxx //192.168.2.203/data/contents/IT事業/04.商品画像/【全カラー別画像各種サイズマスター】/00.リサイズ前 /mnt/y
sudo mount -t cifs -o username=nnnnnnnn,password=xxxxxxxx //192.168.2.203/data/contents/IT事業/04.商品画像/【全カラー別画像各種サイズマスター】/02.main_kyoyu /mnt/z

# 確認
mount -l | grep '/mnt'
df -H

# アンマウント
sudo umount /mnt/y
sudo umount /mnt/z
```

## 02. 判明した問題点とその解決策

1. これは読み取り専用の設定であり、書き込みができない
1. 書き込みをするためには、`--option(-o)` 設定が必要
1. sudo をしないと、`--option(-o)` 設定が有効にならない
1. つまり、シェルスクリプトでも sudo が必要

### sudo したときのパスワードを不要にする

```bash
sudo visudo
# => 最終行に以下を追加する
# => YOURNAME ALL=(ALL) NOPASSWD: ALL
```

### 書き込みを可能にする

```bash
# Windows の共有フォルダをマウントする
sudo mount -t cifs -o username=nnnnnnnn,password=xxxxxxxx,domain=WORKGROUP,rw,uid=1000,gid=1000,file_mode=0755,dir_mode=0755 //192.168.2.203/data/contents/IT事業/04.商品画像/【全カラー別画像各種サイズマスター】/00.リサイズ前 /mnt/y

sudo mount -t cifs -o username=nnnnnnnn,password=xxxxxxxx,domain=WORKGROUP,rw,uid=1000,gid=1000,file_mode=0755,dir_mode=0755 //192.168.2.203/data/contents/IT事業/04.商品画像/【全カラー別画像各種サイズマスター】/02.main_kyoyu /mnt/z
```

#### オプション設定の解説

- `user=arg` ... 接続する際に用いるユーザー名を指定する
- `username=arg` ... smbfsに慣れたユーザーのために長い形式も受け付ける
- `password=arg` ... CIFSのパスワードを指定する
- `domain=arg` ... ユーザーの所属するドメイン名（ワークグループ名）を指定する
- `rw` ... read-writeでマウントする
- `uid=arg` ... サーバーが所有者の情報を提供しない時に、マウントされたファイルシステム上の、すべてのファイルとディレクトリを所有するuidを設定する
- `gid=arg` ... サーバーが所有者の情報を提供しない時に、マウントされたファイルシステム上の、すべてのファイルとディレクトリを所有するgidを設定する
- `file_mode=arg` ... サーバーが CIFS Unix extensions をサポートしていない場合、ファイルモードを上書きする
- `dir_mode=arg` ... サーバーが CIFS Unix extensions をサポートしていない場合、 ディレクトリモードを上書きする
