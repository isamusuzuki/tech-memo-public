# apt パッケージが upgrade されずに kept back された問題

作成日 2021/04/19

## ずいぶん前から発生していた問題

GCP Compute Engine でホストしている Ubuntu 20.04 LTS

```bash
sudo apt update
sudo apt upgrade -y
sudo apt autoremove
# => 0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.

sudo apt upgrade
# => The following packages have been kept back:
# =>   gce-compute-image-packages
# => 0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.

apt list -a gce-compute-image-packages
# => Listing... Done
# => gce-compute-image-packages/focal-updates 20201222.00-0ubuntu2~20.04.0 all [upgradable from: 20190801-0ubuntu4.2]
# => gce-compute-image-packages/now 20190801-0ubuntu4.2 all [installed,upgradable to: 20201222.00-0ubuntu2~20.04.0]
# => gce-compute-image-packages/focal-security 20190801-0ubuntu4.1 all
# => gce-compute-image-packages/focal 20190801-0ubuntu4 all
```

## 解決方法を模索する

```bash
# もう一度インストールしてみる
sudo apt install gce-compute-image-packages

sudo apt update
# => All packages are up to date.

sudo apt upgrade
# => 0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

=> 問題を解決できた
