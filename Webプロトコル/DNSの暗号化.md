# DNS の暗号化

作成日 2019/02/15、更新日 2019/11/07

## 01. DNS の基本的な仕組み

クライアント（スタブリゾルバ）が、DNS サーバ（フルリゾルバ）の UDP ポート 53 番に対してリクエストを投げる

DNS メッセージは、リクエストもレスポンスもそれぞれバイナリ

DNS によるドメイン解決は暗号化されていない

## 02. DNS over HTTPS とは

TCP ポート 443 番に対する HTTPS 通信上で名前解決を行う

DNS メッセージがバイナリな点に変更はなし

「DNS over TLS」という標準もあるが、こちらは TCP ポート 853 番を使う

通常の HTTPS 通信に紛れる DNS over HTTPS のほうが良いように感じるが、
逆に紛れてしまうことでネットワーク管理者から名前解決トラフィックが
見えなくなるデメリットもある

### 03. HTTPS 接続時の SNI とは

SNI = サーバーネームインディケーション

HTTPS のハンドシェイク時に、サーバーに対して、どのドメインへ接続したいかを平文で送っている

「Encrypted SNI」が、TLS1.3 拡張として提案されている

server_name を送信するときに、サーバーからの公開鍵を DNS で取得し、暗号化してからサーバに送信する

この時普通の DNS を使うべきではなく、DNS over HTTPS を使うべき

### SNI とは

virtualhost ... 1 つの IP アドレスと 1 つの TCP ポートで、複数の Web サイトをホスティングする技術

リクエストヘッダに `Host: example.com` がある

HTTPS を使う場合は、この Host フィールドも含め、HTTP ヘッダごと暗号化されてしまう

拡張 TLS 通信のハンドシェイクの一番最初の段階である ClientHello で SNI を使う

server_name の項目にドメイン名を入れる => HTTPS を使っても、どのドメインに接続しているかはネットワーク上の機械で確認できてしまう

Encrypted SNI とは、初回接続時だけ、Host や TLS によらない暗号化を行う

DNS レコードの中に、鍵交換に使うための公開鍵をサーバーが埋め込んでおく

クライントは、まず TLS ハンドシェイクの中で、DNS クエリーで取得した鍵で暗号化を使える

Encrypted SNI は、TLS1.3 の拡張提案でしかないため、実際にブラウザで利用できない

## 04. 「1.1.1.1」とは

IP アドレスで SSL 証明書が表示されていて、びっくりした

[https://1.1.1.1/](https://1.1.1.1/)

Setup on PC

```text
Click Use The Following DNS Server Addresses.
Replace those addresses with the 1.1.1.1 DNS addresses:
For IPv4: 1.1.1.1 and 1.0.0.1
For IPv6: 2606:4700:4700::1111 and 2606:4700:4700::1001
```

### ブラウザのセキュリティチェックを行う

[Cloudflare ESNI Checker](https://www.cloudflare.com/ssl/encrypted-sni/)

- `Secure DNS` ... 安全な DNS を使っているか
- `DNSSEC` ... リゾルバが DNS レスポンスを評価しているか
- `TLS 1.3` ... ブラウザがサポートしているか
- `Encrypted SNI`... ブラウザが SNI を暗号化しているか
