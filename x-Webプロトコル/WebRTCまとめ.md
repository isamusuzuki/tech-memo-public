# WebRTC まとめ

作成日 2019/04/03、更新日 2019/11/07

## 01. WebRTC とは

公式サイト => [https://webrtc.org/](https://webrtc.org/)

WebRTC(Web Real-Time Communication)は、ウェブブラウザの間で特定のプラグインがなくても通信できる API

W3C で提示された草案で、映像、音声、P2P ファイル共有などで活用できる

### P2P 通信が成立するまでに必要なサーバ

- Web サーバ ... 言わずもがな
- STUN サーバ ... NAT 配下にある PC が自分のグローバル IP を取得する
- TURN サーバ ... PC 同士の間に立って、データの中継をする、NAT 越え
- シグナリングサーバ ... 相手 PC に情報を伝える仲介役

## 02. Voluntas のまとめ記事を読む

[仕事で WebRTC](https://gist.github.com/voluntas/379e48807635ed18ebdbcedd5f3beefa)

- 趣味で使いたいなら、これを読んで試してみよ => [Real time communication with WebRTC](https://codelabs.developers.google.com/codelabs/webrtc-web/#0)
- 1:1 や 3 人くらいまでの会議であれば P2P で問題ない
- MCU は 1 部屋 1 コアを利用するくらいの CPU リソースを持っていくシステム
- 仕事として SFU を前提として使っていきたいなら、OSS でがんばるか、商用を利用するかの二択をまず選ぶ

## 03. 「WebSocket / WebRTC の技術紹介」を読む

[https://www.slideshare.net/mawarimichi/websocketwebrtc](https://www.slideshare.net/mawarimichi/websocketwebrtc)

### A. HTTP おさらい

Web における通信の基本
「HTP リクエスト => HTTP レスポンス」の繰り返し

#### HTTP の欠点

1. リソース単位でしか要求・取得ができない
1. 要求をクライアントからしか送れない
1. HTTP ヘッダが大きい

### B. WebSocket の概要

- HTTP と同じく、クライアント・サーバー型
- HTTP と同じく、アプリケーション層のプロトコルで、下位層は TCP である
- HTTP と違って、クライアントとサーバーの双方からデータを送受信できる
- URI スキームは、`ws://~`, `wss://~`
- WebSocket プロトコルに未対応のネットワーク機器が多いので、TLS/SSL の上を`https://~`のフリをしてこっそり流す例が多い
- 通信量は WebSocket が有利（本文が小さいほど、HTTP との差が顕著）

### C. WebRTC の概要

- RTC = Real-Time Communication
- 音声／ビデオチャット、データ通信を使ったリアルタイムコミュニケーション
- WebRTC は単一の規格ではなく、Web で RTC を使うための仕様群
- 表現を変えると、「Web 標準で Skype 等の機能を実現するための仕様群

#### WebRTC を構成する仕様

- PeerConnection ... P2P 接続をするための仕様、下位層は UDP
- DataChannel ... 任意のデータを PeerConnection 経由で送るための仕様
- MediaStream ... ブラウザからマイクやカメラへアクセスするための仕様（正確には WebRTC の一部ではない）
- STUN (Simple Traversal of UDP through NAT) ... NAT の内側にあるノードからグローバルの IP/Port を確認して穴を開ける仕組み
- TURN (Traversal Using Relay NAT) ... NAT の内側にあるノードに対して透過的な経路（トンネル）を提供する、すべての通信をパススルーするので、ノードが増えるほど TURN サーバの帯域が消費される
- ICE (Interactive Connectivity Establishment) ... STUN が使えないときだけ TURN へフォールバックする

## 04. Skyway とは？

NTT コミュニケーションズが運営している WebRTC サーバー

公式サイト => [https://webrtc.ecl.ntt.com/](https://webrtc.ecl.ntt.com/)

無料版あり、API キーの発行が必須

[SkyWay の通信モデル](https://webrtc.ecl.ntt.com/communication-model.html)

[JavaScript SDK](https://webrtc.ecl.ntt.com/js-sdk.html)

[WebRTC Gateway for SkyWay](https://github.com/skyway/skyway-webrtc-gateway) ... IP 通信ができるすべてのデバイスをサポートする。ブラウザの中にある WebRTC エンジンはブラウザの API を介してしか操作できない。ブラウザ上の JavaScript からの操作が前提。外部のプログラムから自由に操作できる、むき出しの WebRTC エンジン

## 05. WebSocket おさらい

### 2 つのスキーマ

- `ws://` ... WebSocket 接続用の新しい URL スキーマ
- `wss://` ... セキュリティで保護された WebSocket 接続

### WebSocket を実装する

```js
var connection = new WebSocket("ws://html5rocks.websocket.org/echo", [
  "soap",
  "xmpp"
]);

connection.onopen = function() {
  connection.send("Ping");
};

connection.onerror = function(error) {
  console.log("WebSocket Error " + error);
};

connection.onmessage = function(e) {
  console.log("Server: " + e.data);
};

connection.send("your message");
```

### getUserMedia() とは

getUserMedia()は、[Media Capture and Streams](https://w3c.github.io/mediacapture-main/) という仕様の中にある API

この API を使うことで、端末のカメラやマイクといった入力ストリームを JavaScript から取得・操作できる

```js
navigator.mediaDevices
  .getUserMedia({ video: true, audio: true })
  .then(stream => {
    // ...
  })
  .catch(err => {
    // ...
  });
```

Promise を返す API となっており、resolve された場合は、MediaStream が取得できる

reject された場合は、一般的なほかの Promise と同じくエラーが渡され、それでエラーハンドリングをすることになる
