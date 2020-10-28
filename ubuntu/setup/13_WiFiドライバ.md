# Ubuntu で WiFi のドライバーをインストールする

作成日 2020/05/30

## TP-Link Archer T9UH 無線 LAN 子機

- 対応規格: 802.11ac/n/a/g/b
- 転送速度: 1300Mbps (11ac), 450Mbps (11n)
- インターフェース: USB 3.0

`lsusb` したら以下のように表示されたが、WiFi のアダプターとしては認識されなかった

```text
Bus 001 Device 005: ID 2357:0106 TP-Link Archer T9UH v1 [Realtek RTL8814AU]
```

## RTL8814AU ドライバーをインストールする

解説記事 => [How to Install RTL8814AU Wireless Driver in Ubuntu 19\.10 \| UbuntuHandbook](http://ubuntuhandbook.org/index.php/2019/11/install-rtl8814au-driver-ubuntu-19-10-kernel-5-13/)

```bash
sudo apt install dkms
git clone https://github.com/aircrack-ng/rtl8812au.git
cd rtl8812au
sudo ./dkms-install.sh
sudo reboot
```

[aircrack\-ng/rtl8812au: RTL8812AU/21AU and RTL8814AU driver with monitor mode and frame injection](https://github.com/aircrack-ng/rtl8812au)

dkms = Dynamic Kernel Module Support\
カーネルのソースツリーの外にソースが存在する Linux カーネルモジュールの生成を可能にするフレームワークのこと

```bash
dkms status
# => rtl8812au, 5.6.4.2, 5.4.0-31-generic, x86_64: installed
```
