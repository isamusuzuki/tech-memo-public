# TCP/IP プロトコルスイート

作成日 2019/01/29、更新日 2019/11/05

## 01. TCP/IP モデルの 4 階層

| Layer              | Protocols                                                                        |
| ------------------ | -------------------------------------------------------------------------------- |
| アプリケーション層 | DHCP, DNS, FTP, HTTP, IMAP, IRC, LDAP, POP, SMTP, SNMP, SSH, TFTP, TLS/SSL, XMPP |
| トランスポート層   | TCP, UDP                                                                         |
| インターネット層   | IPv4, IPv6, ICMP, ICMPv6, IGMP, IPsec                                            |
| リンク層           | ARP, OSPF, SPB, L2TP, PPP, MAC                                                   |

## 02. TCP プロトコルと UDP プロトコル

### TCP プロトコル

- TCP は、接続前に通信開始の準備（ハンドシェイク）があり、1.5 RTT 分の時間がかかる
- TCP は、送信メッセージにシーケンス番号があり、順序が入れ替わりしても、並び替えが可能
- 受信側は、メッセージを受け取り後、シーケンス番号とサイズ合計を確認応答番号として返信する
- 送信側は、確認応答番号が受け取れないと再送信する

### UDP プロトコル

- UDP はノンブロッキングなプロトコルで、TCP ハンドシェイクがない
- マルチキャストとブロードキャストをサポートしている

### RTT (Round Trip Time) とは

TCP での通信に関して RTT は、セグメント送信と ACK 受信の間の時間を計測することで計算する

### ACK とは、NAK とは

- ACK (Acknowledgement) ... 肯定応答
- NAK (Nagative-Acknowledgement) ... 否定応答

### 3 ウェイ・ハンドシェイクとは

TCP などで使用されている、接続を確立するための手順

1. 通信の要求者が相手に SYN パケットを送信する
1. SYN パケットを受けとった通信相手は、SYN ACK パケットを送信する
1. SYN ACK パケットを受け取った要求者は、接続開始を表す ACK パケットを送信し、通信相手との通信を開始する

## 03. Ping で使われるプロトコル

- ICMP = Internet Control Message Protocol
- Ping で確認できるのは、IP レベルまで（「インターネット層」まで）
- 「トランスポート層」や「アプリケーション層」の確認はできない
- 送信元は、タイプ 8 Echo Request の ICMP パケットを宛先に送る
- 宛先ノードは、タイプ 0 Echo Reply の ICMP パケットを送信元に送り返す

## 04. ARP とは

ARP = Address Resolution Protocol

IP アドレスから Ethernet の MAC アドレスの情報を取得するプロトコル

LAN で接続されたコンピュータ間で通信するためには、
IP パケットは下位のレイヤで L2 ヘッダが付加された上で伝送されることから、
MAC アドレスの情報が必要

IP アドレスと MAC アドレスは自動的な関連付けがないので、
ARP で MAC アドレスを得る必要がある

- ARP リクエスト ... 同じセグメントの全ての端末にブロードキャストされる
- ARP リプライ ... ARP リクエストの中身の IP アドレスをもつ端末がユニキャストで送信される

### RARP とは

RARP = Reverse Address Resolution Protocol

自分の IP アドレスを知るために使う
