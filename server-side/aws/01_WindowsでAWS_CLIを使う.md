---
tags: aws
---

# Windows で AWS CLI を使う

作成日 2019/11/18

## Windows 10 に AWS CLI をインストールする

```bash
# Chocolateyを使ってPythonをインストールする
cinst -y python

# Pythonのバージョンを確認する
python --version
#=> Python 3.6.3
pip --version
#=> pip 9.0.1 from C:\python36\lib\site-packages (python 3.6)

# AWS CLIをインストールする
pip install awscli

# AWS CLIをアップグレードする
pip install --upgrade awscli

# AWS CLIのバージョンを確認する
aws --version
#=> aws-cli/1.11.168 Python/3.6.3 Windows/10 botocore/1.7.26
```

### 注意点

-   PowerShell で実行すると「ファイルの関連付けが見つからない」エラーが出る
-   Git Bash で実行すればエラーは出ない

### AWS CLI を設定する

```bash
aws configure --profile <your-profile-name>
#=> AWS Access Key ID [None]: <your-access-key-id>
#=> AWS Secret Access Key [None]: <your-secret-access-key>
#=> Default region name [None]: ap-northeast-1
#=> Default output format [None]: json

# 設定ファイルは2つに別れる
less ~/.aws/credentials
# [<your-profile-name>]
# aws_access_key_id = <your-access-key-id>
# aws_secret_access_key = <your-secret-access-key>

less ~/.aws/config
# [<your-profile-name>]
# output = json
# region = ap-northeast-1
```

### 注意点

-   アクセスキーはどこにもメモしない。使い回さずに必要になったら新規作成すること

## EC2 インスタンスを操作する

```bash
# create.sh
name=robotgon1
aws ec2 run-instances --region ap-northeast-1 \
    --subnet-id <your-subnet-id> \
    --security-group-ids <your-security-group-id> \
    --image-id ami-a77c30c1 \
    --instance-type t2.micro \
    --key-name <your-key-name> \
    --count 1 \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=${name}]' \
    --no-dry-run

# status.sh
name=robotgon1
status=`aws ec2 describe-instances --region ap-northeast-1 \
    --filter "Name=tag:Name,Values=${name}" \
    --query "Reservations[0].Instances[0].State.Name" \
    --output text`
echo ${status}

# assign-ip.sh
name=robotgon1
ip=<your-elastic-ip>
id=`aws ec2 describe-instances --region ap-northeast-1 \
    --filter "Name=tag:Name,Values=${name}" \
    --query "Reservations[0].Instances[0].InstanceId" \
    --output text`
aws ec2 associate-address --region ap-northeast-1 \
    --instance-id ${id} --public-ip ${ip}

# terminate.sh
name=robotgon1
id=`aws ec2 describe-instances --region ap-northeast-1 \
    --filter "Name=tag:Name,Values=${name}" \
    --query "Reservations[0].Instances[0].InstanceId" \
    --output text`
aws ec2 terminate-instances --region ap-northeast-1 \
    --instance-ids ${id}
```

### 作成した EC2 インスタンスに接続する

```bash
# SSH接続する
ssh -i ~/.ssh/robotgon.pem centos@<your-elastic-ip>

# ファイルをコピーする
scp -i ~/.ssh/robotgon.pem ./something.png centos@<your-elastic-ip>:/home/centos

# あらかじめknown_hostに追加する
ssh-keyscan -H <ip_address or hostname> >> ~/.ssh/known_hosts

# known_hostから削除する
ssh-keygen -R <ip_address or hostname>
```

## S3 を操作する

```bash
# ローカルのファイルをS3バケットにコピーする
cd Downloads
aws s3 cp something.png s3://<your-packet-name>/something.png --profile <your-profile>

# ローカルのディレクトリをまとめてS3バケットにコピーする
cd Downloads
aws s3 cp ./<folder-name>/ s3://<your-packet-name>/ --recursive --exclude "*DS_Store" --profile <your-profile> --dryrun
# --exlcude "*DS_Store"で不要ファイルは取り除く
# 最初は--dryrunオプションをつけて実行し、何が起こるかを確認すること

# ローカルのフォルダとS3バケットを同期させる
aws s3 sync ./<folder-name>/ s3://<your-packet-name>/<folder-name> --profile <your-profile-name> --region ap-northeast-1
```

## SSL 証明書を管理する

新たに取得した署名を cer フォルダに保存する

-   `c:\cer\ssl_cert.txt`
-   `c:\cer\intermediate.txt`
-   `c:\cer\private_key.txt`

```bash
# 新しい証明書を追加する
cd /c/cer
aws iam upload-server-certificate
  --server-certificate-name <your-defined-name>
  --certificate-body file://ssl_cert.txt
  --private-key file://private_key.txt
  --certificate-chain file://intermediate.txt
  --profile <your-profile-name>
#=> 成功したら、JSONが表示される

# 登録済みの証明書の一覧を表示する
aws iam list-server-certificates --profile <your-profile-name>
#=> JSONのリストが表示される

# 古い証明書を削除する
aws iam delete-server-certificate
  --server-certificate-name <your-defined-name>
  --profile <your-profile-name>
```

### 証明書を入れ替える

#### Elastic Beanstalk の場合

AWS コンソール ＞ コンピューティング ＞ Elastic Beanstalk \
目的のアプリケーションの目的の環境をクリック \
左枠 ＞ 設定 \
右枠 ＞ ネットワーク階層 ＞ ロード・バランシング ＞ 歯車 \
SSL 証明書 ID を更新して、新しいものを選択し、ページ右下の適用ボタンをクリック \
ブラウザでサイトを開き、SSL 証明書の有効期限が延長されたことを確認する

#### Load Balancer の場合

AWS コンソール ＞ コンピューティング ＞ EC2 \
左枠 ＞ ロードバランシング ＞ ロードバランサー \
右枠 ＞ リスナータブ ＞ 編集ボタンをクリック \
「リスナーの編集」ダイアログが登場 ＞ HTTPS プロトコル ＞ SSL 証明書カラム ＞ 変更リンク \
「証明書の選択」ダイアログが登場 ＞ 証明書を選択する ＞ 保存ボタン ＞ 保存ボタン \
ブラウザでサイトを開き、SSL 証明書の有効期限が延長されたことを確認する
