# Fail メッセージが表示されないようにする

作成日 2020/11/02

## 問題点

サーバーに SSH 接続すると、以下のメッセージが出るようになった

```text
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings
```

## 解決方法

```bash
sudo rm /var/lib/ubuntu-release-upgrader/release-upgrade-available
sudo /usr/lib/ubuntu-release-upgrader/release-upgrade-motd
```

## 調べてみてわかったこと

ログインしたとき、最初に表示されるメッセージのことを `motd` という

motd = message of the day

Ubuntuの場合、motd は `/etc/update-motd.d/` フォルダにある

上のコマンドは、`/etc/update-motd.d/91-release-upgrade` に関連するメッセージをいったん削除し、更新している模様
