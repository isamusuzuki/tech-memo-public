# GCP 署名期限切れ

作成日 2021/04/01、更新日 2021/04/06

## 2021/04/01 問題発生

```bash
sudo apt update
# => The following signatures were invalid: EXPKEYSIG 6A030B21BA07F4FB Google Cloud Packages Automatic Signing Key <gc-team@google.com> The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 8B57C5C2836F4BEB

sudo apt-key list
# => pub   rsa2048 2018-04-01 [SCE] [expired: 2021-03-31]
# =>       54A6 47F9 048D 5688 D7DA  2ABE 6A03 0B21 BA07 F4FB
# => uid           [ expired] Google Cloud Packages Automatic Signing Key <gc-team@google.com>
```

- 署名の有効期限が昨日で切れた
- 新しい署名を取得する必要がある

```bash
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
# => OK
```

- しかし何も変わらない
- 米国はまだ 4/1 になっていないので、誰も気づいていないのか？

## 2021/04/06 対処

### いったんパッケージを削除する

期限切れの鍵を削除する

```bash
# 鍵IDを調べる
sudo apt-key list

# お尻の8文字で指定する
sudo apt-key del BA07F4FB

# 確認する
sudo apt-key list
```

パッケージを削除する

```bash
sudo apt remove google-cloud-sdk
```

apt update したときに巡回するのをやめる

```bash
cd /etc/apt/sources.list.d
sudo rm google-cloud-sdk.list

sudo apt update
```

### もう一度インストールする

[Linux 用のクイックスタート  \|  Cloud SDK のドキュメント  \|  Google Cloud](https://cloud.google.com/sdk/docs/quickstart-linux?hl=ja)

最新のバージョン番号は、以下のページに記載されている

[バージョニングされたアーカイブからのインストール  \|  Cloud SDK のドキュメント  \|  Google Cloud](https://cloud.google.com/sdk/docs/downloads-versioned-archives?hl=ja)

```bash
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-334.0.0-linux-x86_64.tar.gz

tar zxvf google-cloud-sdk-334.0.0-linux-x86_64.tar.gz google-cloud-sdk

./google-cloud-sdk/install.sh
# => ~/.bashrc に必要な設定が書き込まれる
```

いったんシェルをぬけて、変更を反映させる

```bash
which gcloud
/home/i_suzuki/google-cloud-sdk/bin/gcloud

gcloud version
gcloud components list
gcloud components update
gcloud config configurations list
```
