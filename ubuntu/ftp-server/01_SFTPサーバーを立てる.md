# SFTPサーバーを立てる

作成日 2021/04/14

参照記事 => [How to setup SFTP server on Ubuntu 20\.04 Focal Fossa Linux \- LinuxConfig\.org](https://linuxconfig.org/how-to-setup-sftp-server-on-ubuntu-20-04-focal-fossa-linux)

## 参照記事を写経する

```bash
# クラウドにサーバーを立てた場合、SSHサーバーは必ず入っている
# sudo apt install ssh

# 設定ファイルをいじる
sudo vi /etc/ssh/sshd_config
```

設定ファイルの末尾に以下を追加する

```bash
Match group sftp
ChrootDirectory /home
X11Forwarding no
AllowTcpForwarding no
ForceCommand internal-sftp
```

```bash
# SSHサービスを再起動する
sudo systemctl restart ssh

# グループを作成する
sudo addgroup sftp

# ユーザーを作成する
sudo useradd -m sftpuser -g sftp

# 新規ユーザーのパスワードを設定する
sudo passwd sftpuser
```

## sshd_config の設定項目について

参照記事 => [\[OpenSSH\] sshd\_configの設定項目 \- Life with IT](https://l-w-i.net/t/openssh/conf_001.txt)
