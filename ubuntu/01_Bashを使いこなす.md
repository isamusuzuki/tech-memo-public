# Bash を使いこなす

作成日 2020/01/31

## 01. シェルの種類を確認する

| シェルの種類             | 起動方法         | 起動時実行スクリプト |
| ------------------------ | ---------------- | -------------------- |
| ログインシェル           | bash -l(--login) | .bash_profile        |
| インタラクティブシェル   | bash             | .bashrc              |
| 非インタラクティブシェル | bash -c command  | なし                 |

- ログインシェルは`.bashrc`を読まない
- ログインシェルがスクリプトを読む順番は、`.bash_profile`, `.bash_login`, `.profile`
- インタラクティブシェルは、`.bashrc`のみを読む

## 02. 環境変数を組み込む

シェルの種類に応じて、`.bashrc`または`.bash_profile`に以下のコマンドを書き込む

```bash
export DB_HOST_NAME=abcdefg
export DB_USER_NAME=abcdefg
export DB_PASSWORD=abcdefg
export DB_DB_NAME=abcdefg
```

環境変数を確認する

```bash
env | grep DB
```

crontab で実行させるスクリプトは、非インタラクティブシェルとなるので、
`.bashrc`と`.bash_profile`のどちらに書き込んでも、環境変数には組み込まれない
解決方法のひとつとしては、crontab ファイルに直接書き込む方法がある

```bash
DB_HOST_NAME=abcdefg
DB_USER_NAME=abcdefg
DB_PASSWORD=abcdefg
DB_DB_NAME=abcdefg

* 9-23 * * * cd Amachan/kerai; python3 exec.py >> ../temp/cron_kerai.log 2>&1
```
