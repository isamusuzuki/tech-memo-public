# ネットワーク情報を表示するコマンド

作成日 2019/11/06

## 01. 自分のネットワーク情報を調べる

Ubuntu 18.04 に、`ifconfig`コマンドはインストールされていない

```bash
ip address

# 省略形
ip addr
```

## 02. ルーティングテーブルの表示

```bash
netstat -nr
#=> Kernel IP routing table
#=> Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
#=> 0.0.0.0         172.31.32.1     0.0.0.0         UG        0 0          0 eth0
#=> 172.31.32.0     0.0.0.0         255.255.240.0   U         0 0          0 eth0
#=> 172.31.32.1     0.0.0.0         255.255.255.255 UH        0 0          0 eth0

route -n
#=> Kernel IP routing table
#=> Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
#=> 0.0.0.0         172.31.32.1     0.0.0.0         UG    100    0        0 eth0
#=> 172.31.32.0     0.0.0.0         255.255.240.0   U     0      0        0 eth0
#=> 172.31.32.1     0.0.0.0         255.255.255.255 UH    100    0        0 eth0
```

### netstat コマンドのオプション

- `-r` ... ルーティングテーブルを表示する
- `-n` ... コンピュータ名ではなく IP アドレスで、サービス名でなくポート番号で表示する
- `-a` ... すべての接続とリッスンを表示する

## 03. traceroute コマンド

ネットワークの経路を調査する

```bash
# Ubuntuの場合はインストールが必要
sudo apt install traceroute

which traceroute
# => /usr/sbin/traceroute

traceroute www.amazon.co.jp
# => 結果は省略
```

### 結果がチョメチョメになる理由

ルーターが ICMP Time Exceeded に対応していないため \
traceroute は、ICMP Time Exceeded パケットを受け取れなければ、結果を表示できない
