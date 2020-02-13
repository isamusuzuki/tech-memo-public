# Deployment Manager

作成日 2019/10/17

## 01. Deployment Manager とは

テンプレートを使って、クラウドリソースを構築・管理するツール

公式トップ => [https://cloud.google.com/deployment-manager/?hl=ja](https://cloud.google.com/deployment-manager/?hl=ja)

## 02. クイックスタートを写経する

[https://cloud.google.com/deployment-manager/docs/quickstart?hl=ja](https://cloud.google.com/deployment-manager/docs/quickstart?hl=ja)

> 仮想マシンをデプロイします。\
> 仮想マシンをリソースとしてデプロイの構成ファイルに追加します。\
> 構成ファイルを使用してデプロイをします。

YAML 形式で設定ファイルを作成する

`vm.yaml`ファイル

```yaml
resources:
    - type: compute.v1.instance
      name: quickstart-deployment-vm
      properties:
          zone: us-central1-f
          machineType: https://www.googleapis.com/compute/v1/projects/[MY_PROJECT]/zones/us-central1-f/machineTypes/f1-micro
          disks:
              - deviceName: boot
                type: PERSISTENT
                boot: true
                autoDelete: true
                initializeParams:
                    sourceIamge: https://www.googleapis.com/compute/v1/projects/debian-cloud/global/images/family/[FAMILY_NAME]
          networkInterfaces:
              - network: https://www.googleapis.com/compute/v1/projects/[MY_PROJECT]/global/networks/default
                accessConfigs:
                    - name: External NAT
                    - type: ONE_TO_ONE_NAT
```

gcloud コマンドと YAML ファイルを使ってデプロイできる

```bash
# リソースをデプロイする
gcloud deployment-manager deployments\
  create quickstart-deployment\
  --config vm.yaml

# デプロイを確認する
gcloud deployment-manager deployments\
  describe quickstart-deployment

# デプロイを削除する
gcloud deployment-manager deployments\
  delete quickstart-deployment
```
