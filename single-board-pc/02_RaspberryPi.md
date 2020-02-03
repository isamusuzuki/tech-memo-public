# Raspberry Pi をセットアップする

作成日 2019/09/16、更新日 2019/10/27

## 01. NOOBS を入手する

NOOBS は OS インストーラー

ダウンロードサイト => [https://www.raspberrypi.org/downloads/noobs/](https://www.raspberrypi.org/downloads/noobs/)

ZIP ファイルのダウンロードはとても遅いので、Torrent ファイルを選択する

### Chrome OS で、Torrent ファイルを取り扱う

ダウンロードした Torrent ファイルは、ファイルアプリを使って、Linuxのホームフォルダに移す

```bash
cd ~

# aria2 をインストールする
sudo apt install -y aria2

# Torrent ファイルを指定する
aria2c  NOOBS_v3_2_1.zip.torrent

# 入手した ZIP ファイルのハッシュ値を確認する
sha256sum NOOBS_v3_2_1.zip

# 入手した ZIP ファイルを解凍する
mkdir NOOBS_v3_2_1
cd NOOBS_v3_2_1
unzip ../NOOBS_v3_2_1.zip
```

Chrome OS 上の Linux は、リムーバブルディスクにアクセスできない。microSD カードへのコピーは、ファイルアプリで行う

注意点：Chrome OS では、使用中の microSD カードは、読み取り専用になっていて、再フォーマットできない。もう一度フォーマットする場合には、Windows または Ubuntu マシンが必要

## 02. ラズパイ本体をセットアップする

- microSD カードを裏にして、スロットに差す。ノッチはないので、ただ押し込むだけ
- 2 つある HDMI ポートは、USB Type-C ポート寄りのものを使う
- 推奨されている Raspbian を選ぶ
- ユーザー名は相変わらず pi だが、パスワードは自分で決める必要あり

### PC 情報を確認する

```bash
cat /proc/device-tree/model
#=> Raspberry Pi 4 Model B Rev 1.1

cat /proc/cpuinfo
#=> model name : ARMv7 Processor rev 3 (v71)
#=> Hardware   : BCM2835
#=> Revision   : b03111

cat /etc/os-release
#=> PRETTY_NAME="Raspbian GNU/Linux 10 (buster)"
```

## 03. 固定 IP アドレスを設定する

- `/etc/dhcpcd.conf`をいじる
- dhcpd ではなくて、dhcpcd なので要注意
- 5 文字目の c はおそらく「クライアント」

```text
interface eth0
static ip_address=192.168.2.82/24
static routers=192.168.2.1
static domain_name_servers=192.168.2.1
```

## 04. SSH 接続を有効化する

- 設定ツールを起動する　=> `sudo raspi-config`
- `5 Interfacing Options` を選択する
- `P2 SSH` を選択する
- 「有効化しますか？」の質問にハイと答える
- 設定ツールを Finish する
- 再起動する => `sudo shutdown -r now`
- SSH 接続を確認する => `ssh pi@192.168.2.82`

## 05. SSH 接続をパスワードから公開鍵に変更する

自分側で公開鍵を作成し、ラズパイの`~/.ssh/authorized_keys`ファイルに登録する

### 自分側での操作

```bash
ssh-keygen
#=> ファイル名は、デフォルトのまま (~/.ssh/id_rsa) を指定する
#=> パスフレーズは、指定しない（何も入力せずにエンターキーを押す）

# 公開鍵をラズパイに送る
scp ~/.ssh/id_rsa.pub pi@192.168.2.82:/home/pi
```

### ラズパイ側での操作

```bash
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

sudo vi /etc/ssh/sshd_config
#=> PasswordAuthentication yesをnoに変更する

sudo shutdown -r now
```

### 自分側での操作アゲイン

```bash
# SSH接続の確認
ssh pi@192.168.2.82
# => Permission denied (publickey).

ssh pi@192.168.2.82 -i ~/.ssh/pi/id_rsa
# => 接続オーケー
```
