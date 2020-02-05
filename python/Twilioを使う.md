# Twilio を使う

作成日 2019/11/17

## 01. Twilio で SMS を送信する

### 解説記事を探す

[SMS を送信する Alexa スキルを作る（ASK SDK v2 for Node\.js 対応） \- Qiita](https://qiita.com/twilioforkwc/items/b82583de93a26a5f4f02)

> Twilio では、SMS の送信は日本の番号からはできません。アメリカ番号（+1 で始まる番号）を購入する必要があります。

[Twilio を使った SMS の受信と送信元への返信 \- Qiita](https://qiita.com/you03/items/d3a9505894bb77285e68)

### 公式ドキュメントを写経する

[Programmable SMS: アプリケーション内で SMS を送受信する \- Twilio](https://jp.twilio.com/docs/sms)

「はじめてのメッセージを送信しましょう」ボタンをクリック

[メッセージを送信する \- Twilio](https://jp.twilio.com/docs/sms/send-messages)

左枠 ＞ Programmable SMS usage guides ＞ API でメッセージを送信する ＞ Send an SMS with Twilio's API

```python
# Download the helper library from https://jp.twilio.com/docs/python/install
from twilio.rest import Client


# Your Account Sid and Auth Token from twilio.com/console
# DANGER! This is insecure. See http://twil.io/secure
account_sid = 'ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
auth_token = 'your_auth_token'
client = Client(account_sid, auth_token)

message = client.messages.create(
                              body='Hi there!',
                              from_='+15017122661',
                              to='+15558675310'
                          )

print(message.sid)
```

### 自分の環境で動いたスクリプト

.bashrc

```bash
export TWILIO_ACCOUNT_SID=ABCDEFG123456
export TWILIO_AUTH_TOKEN=ABCDEFG123456
```

send_sms.py

```python
import os

from twilio.rest import Client

account_sid = os.environ.get('TWILIO_ACCOUNT_SID')
auth_token = os.environ.get('TWILIO_AUTH_TOKEN')
client = Client(account_sid, auth_token)

message = client.messages.create(
    body='日本語オーケー！ チョベリグー',
    from_='+1206xxxyyyy',
    to='+8180aaaabbbb'
)

print(message.sid)
```

## 02. Twilio で SMS を受信する

### 「SMS Python クイックスタート」を写経する

[Twilio SMS Python クイックスタート \- SMS の送受信 \- Twilio](https://jp.twilio.com/docs/sms/quickstart/python)

```python
from flask import Flask, request
from twilio.twiml.messaging_response import MessagingResponse


app = Flask(__name__)

@app.route('/sms', methods=['GET', 'POST'])
def sms_ahoy_reply():
    resp = MessagingResponse()
    resp.message('Ahoy! Thanks so much for your message.')
    return str(resp)

if __name__ = '__main__':
    app.run(debug=True)
```

これは、XML を返す URL でしかない \
twilio cli は、ngrok トンネルを使って、\
開発サーバーにアクセスできるようにし、\
メッセージを返すようになる\
向こうがどんなメッセージを送ってきたかはまったく見られない

### 「Python で SMS や MMS（日本未対応）を受信、返信する」読む

[Python で SMS や MMS（日本未対応）を受信、返信する方法 \- Twilio](https://jp.twilio.com/docs/sms/tutorials/how-to-receive-and-reply-python)

#### Webhook URL には、どんなデータが来るのか

開発者のアプリケーションへの Twilio からのリクエスト

[TwiML for Programmable SMS \- Twilio](https://jp.twilio.com/docs/sms/twiml)

> Twilio は、`application/x-www-form-urlencoded`形式で HTTP リクエストをユーザーのアプリケーションに送信します。 （Twilio が）このリクエストにパラメーターと値を含めることで、Twilio はユーザーのアプリケーションにデータを送信し、これを使用してレスポンスの前に処理を行うことができます。

### Cloud Functions で Twilio Webhook を受ける

`main.py`

```python
from twilio.twiml.messaging_response import MessagingResponse

def sms_receive(request):
    print('From = ' + request.form['From'])
    print('To = ' + request.form['To'])
    print('Body = ' + request.form['Body'])
    resp = MessagingResponse()
    return str(resp)
```

`requirements.txt`

```
twilio
```

-   Twilio は、フォームを POST 送信してくる
-   Flask の request.form は、ImmutableMultiDict クラスである
-   From, To, Body の 3 つが取れればそれでいい
-   `resp.message()`を入れると返事をしてしまうが、抜けば何もしない

```python
resp = MessagingResponse()
resp.message('Thanks for your message')
return str(resp)
```
