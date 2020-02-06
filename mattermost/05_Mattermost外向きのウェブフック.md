# Mattermost 外向きのウェブフック

作成日 2019/11/21

## 01. ドキュメントを読む

Admin ドキュメント => [Outgoing Webhooks](https://docs.mattermost.com/developer/webhooks-outgoing.html)

開発者ドキュメント => [Outgoing Webhooks](https://developers.mattermost.com/integrate/outgoing-webhooks/)

> Use outgoing webhooks to post automated responses to posts made by your users.
>
> Outgoing webhooks will send an HTTP POST request to a web service and process a response back to Mattermost.

外向きのウェブフックは、ユーザーによる投稿に対して、自動的にレスポンスを投稿する

外向きのウェブフックは、Web サービスに HTTP POST リクエストを送信し、Mattermost への返答を処理する

外向きのウェブフックの起動条件は、次のいずれかまたは両方となる

-   特定のチャンネルに投稿された
-   最初の単語がトリガーワードにマッチした、もしくはそれで始まった

外向きのウェブフックは、公開チャンネルだけで動作する

### Create an Outgoing Webhook

`town-square`チャンネルで、`#build`という書き出しの投稿をすると、ソフトウェアテストを実行することとする

Main Menu > Integration > Outgoing Webhook

`application/x-www-form-urlencoded` か `application/json` を選択

生成されるトークン値をコピーしておく

Mattermost からくる POST データに含まれるもの

-   channel_id
-   channel_name
-   team_domain
-   team_id
-   post_id
-   text=some+text+here
-   timestamp
-   token
-   trigger_word=some&
-   user_id
-   user_name

## 02. 外向きのウェブフックを試す

### 統合機能で「外向きのウェブフック」を追加したときの入力項目

-   タイトル
-   説明
-   コンテントタイプ (application/x-www-form-urlencoded or application/json)
-   チャンネル
-   トリガーワード
-   トリガーとなる条件（最初の単語が「トリガーワードと正確に一致する」、「トリガーワードで始まる」）
-   コールバック URL
-   ユーザー名（投稿する時の）
-   プロフィール画像（投稿する時の）

### 外向きのウェブフックが送信するデータの例

```text
POST /your-url HTTP/1.1
Content-Length: 244
Host: <your-host-name>
Accept: application/json
Content-Type: application/x-www-form-urlencoded

channel_id=abcdefg&
channel_name=town-square&
team_domain=someteam&
team_id=abcdefg&
post_id=abcdefg&
text=some+text+here&
timestamp=1445532266&
token=abcdefg&
trigger_word=some&
user_id=abcdefg&
user_name=somename&
file_ids=abcdefg
```

=> ユーザー名はあるし、テキスト全体もあった
