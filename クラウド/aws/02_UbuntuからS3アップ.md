# Ubuntu から S3 バケットにファイルをアップする

作成日 2019/12/04

## 01. aws-cli をインストールする

```bash
cat /etc/os-release
# => Ubuntu 18.04.3 LTS (Bionic)

python3 --version
#=> Python 3.6.8

sudo apt install python3-pip python3-venv

# 仮想環境をつくる
mkdir aws
cd aws
python3 -m venv .
source bin/activate

pip install awscli

aws --version
#=> aws-cli/1.16.219 Python/3.6.8 Linux/4.15.0-1045-aws botocore/1.12.209
```

## 02. IAM を作成する

| Key                      | Value |
| ------------------------ | ----- |
| ユーザー名               | 適用  |
| アクセスキー ID          | 略    |
| シークレットアクセスキー | 略    |
| 直接アタッチ済みポリシー | 適用  |

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::target-bucket-name/*"
    }
  ]
}
```

## 03. 資格情報を設定する

```bash
aws configure
# => AWS Access Key ID: xxxx
# => AWS Secret Access Key: xxxx
# => Default regions name: ap-northeast-1
# => Default outout format: json
```

## 04. ファイルをアップロードする

```bash
aws s3 cp test.txt s3://target-bucket-name/test.txt
```
