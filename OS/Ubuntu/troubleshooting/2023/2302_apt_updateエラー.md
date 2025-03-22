# `apt update` でエラーが発生

作成日 2023/02/01

## GCP 上の VM インスタンスで問題発生

```bash
sudo apt update
# Err:4 https://packages.cloud.google.com/apt google-cloud-monitoring-focal-all InRelease
#   The following signatures couldn't be verified because the public key is not available: NO_PUBKEY B53DC80D13EDEF05
```

=> `http://packages.cloud.google.com/apt`のキーに問題があるようだ

## 解決策を探る

[gcloud - GPG error: http://packages.cloud.google.com/apt EXPKEYSIG 3746C208A7317B0F - Stack Overflow](https://stackoverflow.com/questions/49582490/gpg-error-http-packages-cloud-google-com-apt-expkeysig-3746c208a7317b0f)

最新のキーを取得すればいいようだ

```bash
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

=> 問題は解消された
