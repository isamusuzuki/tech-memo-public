# Bash スクリプト

作成日 2019/07/25

## 01. 変数を使う

-   変数名=値 ... イコールの前後にスペースを空けてはいけない
-   変数名="値" ... 値の前後にダブルクォートがあってもなくても同じ
-   変数名=`コマンド` ... コマンドの実行結果を挿入するときはアクサングラーブ(``)で囲む

### 変数を参照する

`${変数名}` ... 括弧は誤った解釈を防ぐため

```bash
ip=13.114.58.12
echo ${ip}
```

### 変数を使った複雑なスクリプト例

AWS CLI を使って現在のサーバーの IP アドレスを調べ、\
同時にその戻り値を変数とし、\
そのサーバーからエラーログを取得する

```bash
ip=`aws ec2 describe-instances --region ap-northeast-1 \
--filter "Name=tag:Name,Values=swat2-web-env1" \
--query "Reservations[0].Instances[0].PublicIpAddress" \
--output text`
echo ${ip}
scp -i ~/.ssh/swat2.pem ec2-user@${ip}:/var/log/nginx/error.log ~/Desktop
```

### 特殊な変数

-   `$?` ... 直前に実行されたコマンドの終了ステータス、ゼロかイチ
-   `$#` ... 引数の個数
-   `$0` ... 自分自身のファイル名
-   `$1` ~ `$n` ... 引数

## 02. ユーザーに入力させる

一番簡単な例

```bash
echo Hello, who am I talking to?
read varname
echo It\'s nice to meet you, $varname
```

複数の入力を求める

```bash
read -p 'Username: ' uservar
read -sp 'Password: ' passvar
echo
echo Thankyou $uservar we now have your login details
```

## 03. if 文を使う

```bash
if grep -qs '/mnt/ichigao' /proc/mounts; then
echo "ichigao is already mounted."
else
if [ -e /mnt/ichigao ]; then
echo "ichigao directory exists."
else
sudo mkdir -p /mnt/ichigao
sudo chmod 777 /mnt/ichigao
echo "ichigao directory is created."
fi
sudo mount -t cifs //10.0.0.50/ichigao /mnt/ichigao -o guest
echo "ichigao is now mounted."
fi
```

-   `grep -qs "xxx" /file` ... テキスト内のその文言があるか
-   `[ -e /file ]` ... ファイルやディレクトリが存在するか（括弧前後のスペースは必須）

### if 文を使ったスクリプト例

SSH 接続する先を切り分ける

```bash
#!/bin/bash

# 引数の数が1個でなければエラー
if [ $# -ne 1 ]; then
    echo "need one argument to run" 1>&2
    exit 1
fi

if [ $1 = "tt" ]; then
    echo "Connect to tt staging server"　1>&2
    ssh -i ~/.ssh/secret.pem ec2-user@100.100.100.100 ./something.sh
elif [ $1 = "tm" ]; then
    echo "Connect to tm staging server" 1>&2
    ssh -i ~/.ssh/secret.pem ec2-user@100.100.100.101 ./something.sh
elif [ $1 = "prod" ]; then
    echo "Connect to production server" 1>&2
    ssh -i ~/.ssh/secret.pem ec2-user@100.100.100.102 ./something.sh
else
    echo "select one argument from [tt, tm, prod]" 1>&2
fi
```

出来上がったスクリプトを使う

```bash
./dosomething.sh tt
./dosomething.sh tm
./dosomething.sh prod
```

## 04. エラーを出力させない

```bash
echo 'test' > /dev/null 2>&1
```

標準出力と標準エラー出力を`/dev/null`にリダイレクトする\
`/dev/null`に対して出力されたものはすべて破棄される\
`&1`とすることで、2（標準エラー出力）の出力先を 1（標準出力）と同じにしている

### 応用例

```bash
# ファイルの中身を空にする
cat /dev/null > test.txt
```
