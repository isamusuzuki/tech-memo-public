# Ubuntu で Bluetooth を使う

作成日 2020/05/26

## Linux 対応の USB Bluetooth ドングルを探す

### 参考記事 1

[Best USB Bluetooth Adapters that are Linux\-compatible](https://www.addictivetips.com/ubuntu-linux-tips/best-usb-bluetooth-adapters/)

These are Linux-campatible

- Plugable USB Bluetooth 4.0 Low Energy Micro Adapter (plugable)
- ideapro USB Bluetooth adapter (CSR V4.0)
- Kinivo BTD-400 Bluetooth USB Adapter (KINIVO)
- ZEXMTE Bluetooth USB adapter (ZEXMTE)

### 参考記事 2

[9 Best Bluetooth Adapters in 2020 \- For Windows, Mac, Linux \- The Tech Lounge](https://www.thetechlounge.com/best-bluetooth-adapter/)

These are Linux-campatible

- PLUGABLE USB BLUETOOTH ADAPTER
- KINIVO BTD-400 BLUETOOTH USB ADAPTER

### 結果報告

2020/05/17 Amazon で「Zexmte Bluetooth 4.0 USB ドングル」を 1,480 円で注文した

`lsusb` したら、以下のように表示された

```text
Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
```

Applications ＞ Settings ＞ 左枠 ＞ Bluetooth ＞ 問題なく使うことができた
