# Mattermost サーバーを構築する

作成日 2019/06/05

"Installing Mattermost on Ubuntu 18.04 LTS"を読みながら実行した

[https://docs.mattermost.com/install/install-ubuntu-1804.html](https://docs.mattermost.com/install/install-ubuntu-1804.html)

## 01. AWS EC2 インスタンスを作成する

-   AWS マネジメントコンソール ＞ EC2
-   左枠 ＞ インスタンス ＞ 右枠 ＞ 「インスタンスの作成」ボタン
-   手順 1 ページ（AMI） ＞ AWS Marketplace から Ubuntu を検索 ＞ `Ubuntu 18.04 LTS - Bionic` ＞ Continue ボタン
-   手順 2 ページ（インスタンスタイプ） ＞ `t3.small` ＞ 次の手順
-   手順 3 ページ（インスタンス詳細） ＞ デフォルトのサブネット ＞ 次の手順
-   手順 4 ページ（ストレージ） ＞ 8GB のまま ＞ 次の手順
-   手順 5 ページ（タグ） ＞ なにもいじらず ＞ 次の手順
-   手順 6 ページ（セキュリティグループ） ＞ 既存を選択 ＞ `eg-sg-tyo-mattermost` ＞ 確認と作成
-   手順 7 ページ（確認） ＞ 起動ボタン
-   新しいキーペアの作成 ＞ `eg-mattermost` ＞ 作成開始
-   インスタンス ID = `i-01a46a6ceb2ab31bd`

### Elastic IP の割り当て

-   Elastic IP = `3.113.153.181`
-   アドレスの関連付け ＞ `i-01a46a6ceb2ab31bd` ＞ 関連付け

### SSH 接続のセットアップ

自分の PC で Git Bash を開く

```bash
cd ~

# 接続を確認する
ssh ubuntu@3.113.153.181 -i ~/.ssh/aws/eg-mattermost.pem

touch conn_mm.sh
vi conn_mm.sh
#=> 上の1行をコピペする

# 再度接続を確認する
./conn_mm.sh
```

これ以降は、EC2 インスタンスに SSH 接続しているものとする

## 02. OS の更新

```bash
# 最新の状態にする
sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade -y
sudo apt autoremove -y

# OSのバージョンを確認する
cat /etc/os-release
#=> PRETTY_NAME="Ubuntu 18.04.2 LTS"
```

## 03. MySQL のインストール

```bash
# パッケージをインストールする
sudo apt install mysql-server

# MySQLのバージョンを確認する
mysql --version
#=> mysql  Ver 14.14 Distrib 5.7.26, for Linux (x86_64) using  EditLine wrapper

# rootのパスワードを設定する
sudo mysql_secure_installation
#=> いつものパスワードをrootにセットする

# rootでログインする
sudo mysql
```

MySQL プロンプト下で実行する

```sql
create user 'mmuser'@'%' identified by 'mmuser-5963';
create database mattermost;
grant all privileges on mattermost.* to 'mmuser'@'%';
exit
```

ターミナルに戻る

```bash
# DBへの接続を確認
mysql -u mmuser -p mattermost
```

## 04. Mattermost アプリのインストール

```bash
# 最新版をダウンロードする
wget https://releases.mattermost.com/5.11.0/mattermost-5.11.0-linux-amd64.tar.gz

# ファイルを解凍する
tar -xvzf mattermost-5.11.0-linux-amd64.tar.gz

# ファイルを移動させる
sudo mv mattermost /opt

# ストレージディレクトリをつくる
sudo mkdir /opt/mattermost/data

# ユーザーとグループを作成する
sudo useradd --system --user-group mattermost
sudo chown -R mattermost:mattermost /opt/mattermost
sudo chmod -R g+w /opt/mattermost

# 設定ファイルを編集する
sudo vi /opt/mattermost/config/config.json
```

設定ファイルをいじるのは、とりあえず 1 箇所だけ

```json
{
    "SqlSettings": "mmuser:mmuser-5963@tcp(127.0.0.1:3306)/mattermost?charset=utf8mb4,utf8&readTimeout=30s&writeTimeout=30s"
}
```

### Mattermost アプリをテストする

```bash
# 起動してみる
cd /opt/mattermost
sudo -u mattermost ./bin/mattermost
```

"SiteURL must be set"の警告はでたが、とりあえず動いた

`http://3.113.153.181:8065/` => ブラウザでページを表示させることができた

`ctrl+c` でストップする

### Mattermost アプリをデーモン化する

systemd ユニットファイルを設定する

```bash
sudo touch /lib/systemd/system/mattermost.service
sudo vi /lib/systemd/system/mattermost.service
```

以下をファイルにコピペする

```text
[Unit]
Description=Mattermost
After=network.target
After=mysql.service
Requires=mysql.service

[Service]
Type=notify
ExecStart=/opt/mattermost/bin/mattermost
TimeoutStartSec=3600
Restart=always
RestartSec=10
WorkingDirectory=/opt/mattermost
User=mattermost
Group=mattermost
LimitNOFILE=49152

[Install]
WantedBy=mysql.service
```

ターミナルに戻る

```bash
# systemdに新しいユニットをロードさせる
sudo systemctl daemon-reload

# 新しいユニットがロードされていることを確認する
sudo systemctl status mattermost.service

# サービスをスタートさせる
sudo systemctl start mattermost.service

# サービスがスタートしていることを確認する
sudo systemctl status mattermost.service
curl http://localhost:8065

# マシンが起動したらサービスが始まるようにする
sudo systemctl enable mattermost.service
```

## 05. Nginx のインストール

```bash
# パッケージをインストールする
sudo apt install nginx

# バージョンを確認する
nginx -V
#=> nginx version: nginx/1.14.0 (Ubuntu)

# 動作チェック
curl http://localhost
```

### プロキシーを設定する

```bash
sudo touch /etc/nginx/sites-available/mattermost
sudo vi /etc/nginx/sites-available/mattermost
```

以下をファイルにコピペする

```text
upstream backend {
   server 127.0.0.1:8065;
   keepalive 32;
}

proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=mattermost_cache:10m max_size=3g inactive=120m use_temp_path=off;

server {
   listen 80;
   server_name    mattermost.eg-wms.com;

   location ~ /api/v[0-9]+/(users/)?websocket$ {
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection "upgrade";
       client_max_body_size 50M;
       proxy_set_header Host $http_host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
       proxy_set_header X-Frame-Options SAMEORIGIN;
       proxy_buffers 256 16k;
       proxy_buffer_size 16k;
       client_body_timeout 60;
       send_timeout 300;
       lingering_timeout 5;
       proxy_connect_timeout 90;
       proxy_send_timeout 300;
       proxy_read_timeout 90s;
       proxy_pass http://backend;
   }

   location / {
       client_max_body_size 50M;
       proxy_set_header Connection "";
       proxy_set_header Host $http_host;
       proxy_set_header X-Real-IP $remote_addr;
       proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header X-Forwarded-Proto $scheme;
       proxy_set_header X-Frame-Options SAMEORIGIN;
       proxy_buffers 256 16k;
       proxy_buffer_size 16k;
       proxy_read_timeout 600s;
       proxy_cache mattermost_cache;
       proxy_cache_revalidate on;
       proxy_cache_min_uses 2;
       proxy_cache_use_stale timeout;
       proxy_cache_lock on;
       proxy_http_version 1.1;
       proxy_pass http://backend;
   }
}
```

ターミナルに戻る

```bash
# 既存のリンクを削除する
sudo rm /etc/nginx/sites-enabled/default

# 新しいリンクを作成する
sudo ln -s /etc/nginx/sites-available/mattermost /etc/nginx/sites-enabled/default

# NGINXを再起動する
sudo systemctl restart nginx

curl http://localhost
```

`http://mattermost.eg-wms.com/` => ブラウザでページを表示させることができた

セキュリティグループのポート 8065 は閉じておく

## 06. Let's Encrypt をインストールする

### Let's Encrypt のインストールは、certbot の公式ページに従う

公式ページ => [https://certbot.eff.org/](https://certbot.eff.org/)

```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt update
sudo apt upgrade
sudo apt install certbot python-certbot-nginx

sudo certbot --nginx certonly
#=> Enter email address: i_suzuki@everglowtrading.com
#=> (A)gree/(C)ancel: A
#=> (Y)es/(N)o: N
#=> Which names activate for?: mattermost.eg-wms.com
```

結果をメモする

```text
IMPORTANT NOTES:
- Congratulations! Your certificate and chain have been saved at:
  /etc/letsencrypt/live/mattermost.eg-wms.com/fullchain.pem
  Your key file has been saved at:
  /etc/letsencrypt/live/mattermost.eg-wms.com/privkey.pem
  Your cert will expire on 2019-09-03.
```

### Nginx の設定を更新して、SSL 証明書を使うようにする

`/etc/nginx/sites-available/mattermost`ファイルを編集する

(1) 以下の 5 行をまるまる追加する

```text
server {
   listen 80 default_server;
   server_name   mattermost.eg-wms.com ;
   return 301 https://$server_name$request_uri;
}
```

(2) もとからあった server ブロックを改造する

```text
server {
  # listen 80 を削除して 443 に変更
  listen 443 ssl http2;
  server_name    mattermost.eg-wms.com ;

  # まとめてコピペ
  ssl on;
  ssl_certificate /etc/letsencrypt/live/mattermost.eg-wms.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/mattermost.eg-wms.com/privkey.pem;
  ssl_session_timeout 1d;
  ssl_protocols TLSv1.2;
  ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_prefer_server_ciphers on;
  ssl_session_cache shared:SSL:50m;

  # ここからはそのまま
  location ~ /api/v[0-9]+/(users/)?websocket$ {
```

(3) NGINX を再起動する

`sudo systemctl restart nginx`

`https://mattermost.eg-wms.com/` => ブラウザでの表示に成功

## 07. SSL 証明書の自動更新

### Mattermost サーバーの SSL 証明書は勝手に更新されていた

更新が切れる日の 30 日前に、勝手に更新する設定が入っていた

| 証明書 | 有効期間                   |
| ------ | -------------------------- |
| 初回   | 2019/06/05 から 2019/09/03 |
| 今回   | 2019/08/04 から 2019/11/02 |

### 公式サイトの説明

[Certbot \- Ubuntubionic Nginx](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

> The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration.

### 証拠探しをする

```bash
systemctl status certbot.timer
#=> certbot.timer - Run certbot twice daily

sudo less /etc/letsencrypt/renewal/mattermost.eg-wms.com.conf
#=> renew_before_expiry = 30 days
```
