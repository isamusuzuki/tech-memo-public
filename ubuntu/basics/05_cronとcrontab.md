# cron と crontab

作成日 2020/02/26、更新日 2020/09/08

## 01. cron の注意点

- cron 実行時のカレントフォルダは、実行ユーザーのホームフォルダ
- cron 実行時のシェルは、`.bashrc` を読まない
- source コマンドを使うときは、`SHELL=/bin/bash` を書いて、シェルを Bash にする

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

## 03. crontab の書き方例

```text
GOOGLE_APPLICATION_CREDENTIALS=/home/ubuntu/credentials.json

17 * * * * cd <project-name>; python3 main.py >> temp/cron1.log 2>&1
31 3 * * * cd <project-name>; python3 shuppin.py shousai >> ../temp/cron2.log 2>&1
* 9-23 * * * cd <project-name>; python3 exec.py >> ../temp/cron3.log 2>&1
```

## 04. unix-cron 形式（5 桁の数字）について

| No  | field        | available |
| :-: | ------------ | --------- |
|  1  | min          | 0-59      |
|  2  | hour         | 0-23      |
|  3  | day of month | 1-31      |
|  4  | month        | 1-12      |
|  5  | day of week  | 0-6       |

- `* * * * *` ... 1 分ごと（最も短いサイクル）
- `0 * * * *` ... 毎時 0 分
- `17 * * * *` ... 毎時 17 分
- `*/10 * * * *` ... 10 分おき（10 で割り切れる数 => 0, 10, 20, 30, 40, 50）
- `0 3 * * *` ... 毎日朝 3 時
- `0 */3 * * *` ... 3 時間おき
- `0 9 * * 1` ... 毎週月曜日の朝 9 時
- `*/5 9-17 * * 1-5` ... 平日 9:00 から 17:55 の間、5 分おき（5 で割り切れる数）
- `3,23,43 9-21 * * *` ... 毎日 9:03 から 21:43 の間、20 分おき
