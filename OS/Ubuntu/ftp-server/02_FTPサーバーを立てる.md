# FTP サーバーを立てる

作成日 2021/04/14

参照記事 => [How to setup FTP server on Ubuntu 20\.04 Focal Fossa Linux \- LinuxConfig\.org](https://linuxconfig.org/how-to-setup-ftp-server-on-ubuntu-20-04-focal-fossa-linux)

## 参照記事を写経する

```bash
# vsftpdをインストールする
sudo apt install vsftpd

# オリジナルファイルのバックアップを取る
sudo mv /etc/vsftpd.conf /etc/vsftpd.conf_orig

# 設定ファイルを作成する
sudo vi /etc/vsftpd.conf
```

設定ファイル

```bash
listen=NO
listen_ipv6=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
pasv_enable=Yes
pasv_min_port=10000
pasv_max_port=10100
allow_writeable_chroot=YES
```

```bash
# FTPトラフィックを許可する
sudo ufw allow from any to any port 20,21,10000:10100 proto tcp

# vsftpサービスを再起動する
sudo systemctl restart vsftpd

# ユーザーを作成する
sudo useradd -m ftpuser
sudo passwd ftpuser

# テストデータを仕込んでおく
sudo bash -c "echo FTP TESTING > /home/ftpuser/FTP-TEST"
```
