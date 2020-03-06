# cron を使う

作成日 2020/02/26、更新日 2020/03/06

## 01. cron の注意点

- cron 実行時のカレントフォルダは、実行ユーザーのホームフォルダ
- cron 実行時のシェルは、`.bashrc` を読まない
  - 環境変数は、crontab の中で設定することができるが、
  - dotenv を使って、実行するスクリプトの中で、環境変数を読み込むことが望ましい

## 02. cron の設定方法

```bash
# デーモンの稼働状況を確認する
systemctl status cron
# => cron.service - Regular background program processing daemon
# => Loaded: loaded
# => Active: active (running)

# ログインしているユーザーのcrontabの内容を表示する
crontab -l

# ログインしているユーザーのcrontabを編集する
crontab -e

# ログインしているユーザーのcrontabを削除する
crontabl -r

# root ユーザーに昇格していれば、他のユーザーのcrontabも表示できる
crontab -u other-user -l
```

### 03. crontab の書き方例

```text
GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/credentials.json

17 * * * * cd <project-name>; python3 main.py >> temp/cron.log 2>&1
```

## 04. crontab 以外の設定方法

/etc/cron.d ディレクトリに、設定ファイルを置くことでも、\
cron ジョブを設定することができる\
この場合は、実行するユーザー名を指定できる（root ユーザーを含む）
