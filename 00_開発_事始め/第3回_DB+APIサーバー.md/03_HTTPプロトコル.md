# HTTPプロトコル

作成日 2026/01/13

## HTTPプロトコルとは

- HTTP = Hypertext Transfer Protocol
- TCPの接続が完了すると、クライアント側からHTTPのファイル取得要求を送信する、サーバー側から応答を1つ返す
- Webページを構成する部品が多数ある場合、それらも全てこの手順を繰り返して取得する
- Webブラウザは複数のHTTP通信（TCPコネクション）を開設して、同時に取得して、通信時間を短縮する

HTTP/1.1以降の規格では、基本的にコネクションはずっとオープンしたままでデータのやりとりを行う

- HTTP/0.9 ... GETメソッドのみ
- HTTP/1.0 ... HEAD/POSTメソッドが追加、ヘッダフィールドが整備、MIMEタイプもサポート
- HTTP/1.1 ... PUT/DELETE/OPTIONS/CONNECTメソッドが追加
- HTTP/2.0 ... 2015年制定

コマンドも応答もすべてテキスト形式なので、通信効率は悪いが人間には理解しやすい

### HTTPメッセージの詳細

HTTPでサーバーとクライアントがやりとりするHTTPメッセージは、基本的に「メッセージヘッダ + 空行(CR+LF) + メッセージボディ」という構造になっている。
リクエストヘッダの 1 行目は、リクエストラインとなる => 要求の書式

#### 要求の書式

「メソッド名 ターゲット HTTP バージョン」

```text
GET /index.html HTTP/1.1
```

#### 応答の書式

「HTTP バージョン ステータスコード 結果フレーズ」

```text
HTTP/1.1 200 OK
HTTP/1.1 301 Moved Permanently
HTTP/1.1 404 Not Found
```

#### HTTP 要求

- GET ... 指定したリソースを取得
- HEAD ... ヘッダ情報だけ取得
- POST ... データを送信
- PUT ... データを送信して書き換え
- DELETE ... 削除
- CONNECT ... プロキシにおけるトンネリング処理
- OPTIONS ... 利用可能なメソッドの一覧を返す
- TRACE ... サーバーの動作を診断

#### HTTP ステータスコード

- 1xx ... Informational
- 2xx ... Successful
- 3xx ... Redirection
- 4xx ... Client Error
- 5xx ... Server Error
