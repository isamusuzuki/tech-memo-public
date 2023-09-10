# IoT 向けプロトコルまとめ

作成日 2019/11/07

## 01. MQTT とは

MQTT = Message Queueing Telemetry Transport

シンプルで軽量、短いメッセージを頻繁に送受信することを想定してつくられた

機械同士が通信を介して情報をやり取りする M2M や、家電や自動車などがインターネットに繋がる IoT を実現するのに適したプロトコルと言われる

プロトコルヘッダが小さい、HTTP が「50 バイト～」なら、MQTT は「2 バイト～」

パブリッシュ・サブスクライブ・モデルを採用

![publish-subscribe-model.png](https://imgur.com/hVWUCjy.png)

1990 年代後半に IBM が考案して開発したプロトコル

### Mosquitto とは

オープンソースの MQTT ブローカー

公式サイト => [https://mosquitto.org/](https://mosquitto.org/)

```bash
# MQTTブローカーをバックグラウンドで実行する
mosquitto -d

# 別のターミナルから、ローカルブローカーに接続して、
# トピックにサブスクライブする
mosquitto_sub -t "dw/demo"

# 別のターミナルから、ローカルブローカーに接続して、
# トピックにメッセージをパブリッシュする
mosquitto_pub -t "dw/demo" -m "hello, world!"
# => サブスクライブしている画面にメッセージが出力される
```

#### Mosquitto の入手方法

[Download \| Eclipse Mosquitto](https://mosquitto.org/download/)

ラズパイや Ubuntu は、メインリポジトリから入手可能

CentOS は、`/etc/yum.repos.d/`にリポジトリ設定ファイルを追加する

```text
[home_oojah_mqtt]
name=mqtt (CentOS_CentOS-7)
type=rpm-md
baseurl=http://download.opensuse.org/repositories/home:/oojah:/mqtt/CentOS_CentOS-7/
gpgcheck=1
gpgkey=http://download.opensuse.org/repositories/home:/oojah:/mqtt/CentOS_CentOS-7/repodata/repomd.xml.key
enabled=1
```

### QoS とは

QoS = Quality of Service

Publisher から Broker まで、Broker から Subscriber までの区間の通信の品質

- QoS0 ... メッセージを最大で 1 回だけ届ける。メッセージが無事に届いたかどうかを確認しない
- QoS1 ... 最低でも 1 回はメッセージを届ける。メッセージが届いたかどうか確認する。受信側から PUBACK を送る
- QoS2 ... 正確に 1 回だけメッセージを届ける。MQTT で一番遅い通信。受信側から PUBREC,PUBCOMP を送る

QoS レベルは、Broker が対応している QoS レベルしか使用できない。Publisher が指定した QoS より上のレベルで通信を行うことはできない

## 02. CoAP とは

CoAP = Constrained Application Protocol

UDP ベースの簡易 HTTP を目指したプロトコル

LWM2M がこの上で規定されている

公式サイト => [http://coap.technology/](http://coap.technology/)

### CoAP のセキュリティ懸念

CoAP は HTTP にとてもよく似ているが、TCP パケット上ではなく、UDP 上で動作する。

HTTP がクライアントとサーバ間のデータやコマンド（GET, POST, CONNECT など）転送に利用されるのと同様に、
CoAP も同じようなマルチキャストおよびコマンド転送機能を持つが、HTTP ほど多くのリソースを必要としない。

UDP ベースの他のプロトコルと同じく、DDoS 攻撃の「IP スプーフィング」と「パケット増幅」に対して脆弱性を持つ

## 03. MQTT vs HTTP vs CoAP

- CoAP ... 簡易 HTTP を目指したプロトコル、UDP ベース
- HTTP ... 双方向通信が必要ならこれでもいい
- MQTT ... IoT/M2M なら、なにも考えずにこれ
