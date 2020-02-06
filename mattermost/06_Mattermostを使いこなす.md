# Mattermost を使いこなす

作成日 2019/11/25

## 01. 標準装備の絵文字

もちろん HackMD でもサポートされている

-   :white_check_mark: `:white_check_mark:`
-   :bangbang: `:bangbang:`
-   :thumbsup: `:thumbsup:`
-   :no_good: `:no_good:`
-   :thumbsdown: `:thumbsdown:`
-   :point_right: `:point_right:`
-   :book: `:book:`
-   :books: `:books:`

[🎁 Emoji cheat sheet for GitHub, Basecamp, Slack & more](https://www.webfx.com/tools/emoji-cheat-sheet/)

## 02. Mattermostの内部を勉強する

### フォルダ構造

```text
--/opt/mattermost/
    |--ENTERPRISE-EDITION-LICENSE.txt
    |--NOTICE.txt
    |--README.md
    |--bin/
    |--client/
    |--config/
    |--data/
    |--fonts/
    |--i18n/
    |--logs/
    |--plugins/
    |--prepackaged-plugins/
    `--templates/
```

## 03. Mattermost Slash コマンド

### ドキュメントを読む

Admin ドキュメント => [Slash Commands](https://docs.mattermost.com/developer/slash-commands.html)\
開発者ドキュメント => [Slash Commands](https://developers.mattermost.com/integrate/slash-commands/)

### 統合機能で「Slash コマンド」を追加したときの入力項目

-   タイトル
-   説明
-   コマンドトリガーワード
-   リクエスト URL
-   返信ユーザー名
-   応答アイコン
-   自動補完

### Slash コマンドが送信するデータの例

```text
POST / HTTP/1.1
Host: 127.0.0.1:8080
User-Agent: mattermost-5.9.0
Content-Length: 548
Accept: application/json
Authorization: Token nezum4kpu3faiec7r7c5zt6tfy
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip

channel_id=abcdefg&
channel_name=town-square&
command=%2Fexample&
response_url=http%3A%2F%2F10.0.0.5%3A8065%2Fhooks%2Fcommands%2Fabcdefg&
team_domain=rrrr&
team_id=abcdefg&
text=&
token=abcdefg&
trigger_id=abcdefg&
user_id=abcdefg&
user_name=tester
```

## 04. Mattermost で SSO を実現する

### Keycloak で実現した記事を読む

[Keycloak で Mattermost\(Team Edition\)に SSO ログインする \- Qiita](https://qiita.com/wadahiro/items/8b118c34aae904353865)

> 残念ながら SAML による SSO に関しては\
> 有償版の Enterprise E20 でないと提供されていません。\
> Mattermost は GitLab を使ってのログイン機能であれば全エディションで\
> 利用可能なため...\
> 実は Mattermost の GitLab ログイン機能は、連携の際の\
> HTTP リクエスト/レスポンスを見ると、\
> 単に OAuth2.0 の認可コードフローで ID 連携している\
> ということが分かります。

#### Keycloak に Mattermost 用のクライアントを登録する

Keycloak に SSO 対象のアプリケーションを登録する場合は、\
クライアントと呼ばれるものを登録する必要があります。\
また、Keycloak にはこのクライアントのプロトコルとして\
OpenID Connect と SAML の二種類があります。\
Mattermost の GitLab ログイン機能は OAuth2 を利用しているため、\
作成するクライアントのプロトコルには`openid-connect`を選択します

#### Mattermost に GitLab 認証を設定する

Mattermost の System Console > AUTHENTICATION > GitLab から\
連携設定を有効化します。\
用意した Client ID と Secret をここで設定します。\
また、ここで OAuth2 のエンドポイント URL 設定を行うのですが、\
GitLab Site URL しか入力できず、\
エンドポイント単位に細かく設定できません。\
そのため、Keycloak が提供する形式の OAuth2 エンドポイント URL を\
設定することができません。

Mattermost では GitLab ログインの設定は config.json ファイルで\
設定できるようになっています。\
GUI で設定せず、このファイルを直接編集することで\
細かく OAuth2 エンドポイント URL を設定することができます。

```json
{
    "GitLabSettings": {
        "Enable": true,
        "Secret": "abcdefg123456",
        "Id": "mattermost",
        "Scope": "",
        "AuthEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/auth",
        "TokenEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/token",
        "UserApiEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/userinfo"
    }
}
```

#### 動作確認

Mattermost の画面にアクセスし、画面下部に表示される\
Sign in with から GitLab をクリックします。\
GitLab の代わりに Keycloak を設定しているため、\
Keycloak のログイン画面が表示されますので、\
登録しておいたテスト用ユーザでログインします。\
ログイン後 Mattermost にリダイレクトで戻りますが、\
OAuth2 の設定が正しくされていれば無事に\
SSO ログインできているはずです。
