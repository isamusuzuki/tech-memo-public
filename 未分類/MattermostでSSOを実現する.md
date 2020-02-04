---
tags: sso, mattermost
---

# MattermostでSSOを実現する

作成日 2019/11/26

## 01. Keycloakで実現した記事を読む

[KeycloakでMattermost\(Team Edition\)にSSOログインする \- Qiita](https://qiita.com/wadahiro/items/8b118c34aae904353865)

>残念ながらSAMLによるSSOに関しては\
>有償版のEnterprise E20でないと提供されていません。\
>MattermostはGitLabを使ってのログイン機能であれば全エディションで\
>利用可能なため...\
>実はMattermostのGitLabログイン機能は、連携の際の\
>HTTPリクエスト/レスポンスを見ると、\
>単にOAuth2.0の認可コードフローでID連携している\
>ということが分かります。

### KeycloakにMattermost用のクライアントを登録する

KeycloakにSSO対象のアプリケーションを登録する場合は、\
クライアントと呼ばれるものを登録する必要があります。\
また、Keycloakにはこのクライアントのプロトコルとして\
OpenID ConnectとSAMLの二種類があります。\
MattermostのGitLabログイン機能はOAuth2を利用しているため、\
作成するクライアントのプロトコルには`openid-connect`を選択します

### MattermostにGitLab認証を設定する

MattermostのSystem Console > AUTHENTICATION > GitLabから\
連携設定を有効化します。\
用意したClient IDとSecretをここで設定します。\
また、ここでOAuth2のエンドポイントURL設定を行うのですが、\
GitLab Site URLしか入力できず、\
エンドポイント単位に細かく設定できません。\
そのため、Keycloakが提供する形式のOAuth2エンドポイントURLを\
設定することができません。

MattermostではGitLabログインの設定はconfig.jsonファイルで\
設定できるようになっています。\
GUIで設定せず、このファイルを直接編集することで\
細かくOAuth2エンドポイントURLを設定することができます。

```json=
{
    "GitLabSettings": {
        "Enable": true,
        "Secret": "694ce9b7-9bed-4961-9ad2-8c680a9e1daa",
        "Id": "mattermost",
        "Scope": "",
        "AuthEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/auth",
        "TokenEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/token",
        "UserApiEndpoint": "http://sso.example.org/auth/realms/sample/protocol/openid-connect/userinfo"
    }
}
```

### 動作確認

Mattermostの画面にアクセスし、画面下部に表示される\
Sign in withからGitLabをクリックします。\
GitLabの代わりにKeycloakを設定しているため、\
Keycloakのログイン画面が表示されますので、\
登録しておいたテスト用ユーザでログインします。\
ログイン後Mattermostにリダイレクトで戻りますが、\
OAuth2の設定が正しくされていれば無事に\
SSOログインできているはずです。

