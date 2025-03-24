# SSL/TLS プロトコルまとめ

作成日 2019/01/31、更新日 2019/11/06

## 01. SSL 暗号化通信の仕組み

SSL は、一対の機器の間でデータの暗号化通信を行うことができる、トランスポート層の仕組み（プロトコル）。

SSL 暗号化通信は、対になる 2 つの鍵「共通鍵暗号方式」「公開鍵暗号方式」の仕組みを用いて行われる

![ssl-handshake.png](https://imgur.com/KcdD0ZF.png)

共有鍵（セッションキー）は、クライアント側で生成される

共有鍵は、サーバーとクライアントの双方が対応する、最も強度が高い暗号方式・鍵長が使用される

## 02. TLS (Transport Layer Security) とは

TLS は、SSL(Secure Socket Layer)の次世代規格
1995 年に SSL3.0 がリリースされたが、2014 年に脆弱性が見つかり、2015 年に無効化された
2008 年に TLS 1.2 がリリースされた、これが現在の主流
SSL は、ネットスケープが開発したプロトコル。これを IETF が標準化を行った際に TLS に名称が変更された

TLS では、アプリケーション層のデータは共通鍵方式で暗号化される

## 03. 暗号スイート (Cipher Suites) とは

暗号技術の組み合わせのこと

以下は、CentOS 7.5 で試してみた

```bash
cat /etc/redhat-release
#=> CentOS Linux release 7.5.1804 (Core)

openssl version
#=> OpenSSL 1.0.2k-fips 26 Jan 2017

openssl ciphers
#=> ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:...

openssl ciphers -v 'ALL'
#=> ECDHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AESGCM(256) Mac=AEAD
#=> ECDHE-ECDSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AESGCM(256) Mac=AEAD
#=> ECDHE-RSA-AES256-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AES(256)  Mac=SHA384
```

左から順に、6 列の情報を表示している

1. 暗号スイートの名前 `ECDHE-RSA-AES256-GCM-SHA384`
1. プロトコルのバージョン `TLSv1.2`
1. 鍵交換(KX=KeyeXchange)に使われる暗号化アルゴリズム `Kx=ECDH`
1. 認証(Au=Authentication)に使われる暗号化アルゴリズム `Au=RSA`
1. アプリケーション層の暗号化(Enc=Encryption)に使われる暗号化アルゴリズム `Enc=AESGCM(256)`
1. メッセージの検証に使われるハッシュアルゴリズム (Mac=Message Authentication Code) `Mac=AEAD`

## 04. メッセージ認証コードとは

通信内容の改ざんを検知するための作業は、以下の通り

- データを送信する前に、ハッシュ関数を使って、そのデータから指紋を作る
- データの受信者は、受け取ったデータから指紋を取り出し、
- 同じように、データをハッシュ関数に通して、指紋が一致するかどうかを確認する

この指紋のことを、メッセージ認証コード (Message Autnenticantion Code = MAC) という

## 05. 公開鍵証明書とは

- 公開鍵証明書とは、公開鍵とその所有者の同定情報を結び付けるもの
- 「デジタル証明書」とも呼ばれる
- 公開鍵は単にバイナリデータである => 証明書が、公開鍵と所有者本人を結ぶ
- 通常、公開鍵証明書には公開鍵そのもののデータも含まれている
- PKI (Public Key Infrastructure) = 公開鍵基盤
- CA (Certificate Authority) = 認証局

### 主な認証方式（証明書の種類）

- Domain Validation 認証
- Organization Validation 認証
- Extended Validation 証明書（EV 証明書）

最新のブラウザは、EV 証明書から従来の SSL 証明書より多くの情報を取得する

- アドレスバーが緑色になる
- ウェブサイト所有者のの名称や所在地の要約が専用のラベルに登場する

### 06. X.509 とは

X.509 は、デジタル証明書の規格
認証局が発行する公開鍵証明書は、X.509 が採用されている
以下は、X.509 v3 デジタル証明書の構造

```text
証明書
    バージョン
    通し番号
    アルゴリズムID
    発行者
    有効期間（開始、終了）
    主体者
    主体者の公開鍵情報（公開鍵アルゴリズム、公開鍵）
    発行者の一意な識別子
    主体者の一意な識別子
証明書の署名アルゴリズム
証明書の署名
```

証明書の検証には、デジタル署名が使われる

証明書のデジタル署名をする => 秘密鍵を使って証明書を暗号化して、その暗号文を署名として公開する

証明書のデジタル署名を検証する => 公開鍵を使って署名を復号化して、元の文書と一致していれば検証成功
