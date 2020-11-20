# SSO (Single Sign-On, シングルサインオン）

作成日 2019/11/26

## 01. SSO とは

1 つのログイン ID とパスワードで、複数の Web サービスにログインできる仕組み

![sso.png](https://imgur.com/SJEwQb2.png)

### SSO の仕組み

- エージェント方式 ... Web アプリにエージェントを導入、Cookie を利用
- リバースプロキシ方式 ... リバースプロキシサーバーを設置、そこにエージェントを導入
- 代理認証方式 ... ユーザーの代わりに ID/パスワードを送信
- フェデレーション方式 ... SAML, OpenID Connect を使う

### IDaaS の比較記事を読む

[IDaaS の比較 \(Cognito, Firebase Authentication, Auth0\) \- Qiita](https://qiita.com/ibuki/items/9c47df0cc4e9ee6ca0bc)

> Cognito User Pools, Firebase Authentication, Auth0 の 3 つを比較する

[最近流行り\(?\)の IDaaS についてまとめます \- Qiita](https://qiita.com/osak/items/28eda07e5d0183c6f99a)

> - SAML ... 企業内やより高いセキュリティを求められるシステムで利用する
> - OpenID Connect ... 不特定多数のコンシューマが使うアプリでよく利用される
> - OAuth ... WepAPI などに利用
>
> 複数のアプリがある場合、同じ IDaaS を利用すれば、一回のログインで、すべてのアプリへの遷移が可能
>
> - Okta ... IDaaS 最大手
> - Auzure Active Directory
> - Oracle Identity Cloud Service
> - OneLogin
> - AWS Single Sign-On
> - TrustLogin
> - Auth0

## 02. OpenID Connect (OIDC) とは

[OpenID Connect の始め方 \- Qiita](https://qiita.com/sawadashota/items/d7b794c155d01c319b04)

> OpenID Connect は、OAuth2.0 をベースに作られているので、まずは OAuth2.0 を理解するところから始めましょう。\
> OpenID Connect は、OAuth2.0 に認証レイヤーを追加したものです。\
> もっと言うと、OAuth2.0 で標準化されていない ID 連携の部分を標準化したものです。

[IBM Knowledge Center \- OpenID Connect](https://www.ibm.com/support/knowledgecenter/ja/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/cwlp_openid_connect.html)

> OpenID Connect は、OAuth2.0 プロトコルに基づいて構築された、単純な ID プロトコルおよびオープン・スタンダードです。\
> これにより、クライアント・アプリケーションは、OpenID Connect プロバイダーによって実行された認証を利用して\
> ユーザーの ID を検証できます。\
> OpenID Connect は、認証および許可に OAuth 2.0 を使用し、ユーザーを一意的に識別する ID を作成します。\
> クライアント・アプリケーションは、OpenID Connect プロバイダーから相互運用可能な REST のような方法で、\
> ユーザーに関する基本プロファイル情報を取得することもできます。

- アクセス・トークン ... 保護リソースにアクセスするために使用される資格情報。アクセス・トークンは、クライアントに発行された許可を表すストリングです。
- 許可エンドポイント ... ユーザーの認証および許可を実行するためにクライアントからの許可要求を受け入れる、OpenID プロバイダー上のリソース。許可エンドポイントは、許可コード・フローでクライアントに許可付与 (許可コード) を返します。暗黙的フローでは、許可エンドポイントは、ID トークンおよびアクセス・トークンをクライアントに返します。
- ID トークン ... 認証済みユーザーに関するクレームが含まれている JSON Web トークン (JWT)。
- クレーム ... エンティティーに関して表明された情報。クレームの例としては、電話番号、名、姓などがあります。
- OP ... Open ID Connect プロバイダー、クライアントまたはリライング・パーティー (RP) にクレームを提供できる OAuth 2.0 許可サーバー。
- RP ... リライティングプロバイダー、OpenID Connect クライアントとして構成されている Liberty サーバー、または OpenID プロバイダー (OP) にクレームを要求するクライアント・アプリケーションのいずれか。
- トークン・エンドポイント ... アクセス・トークン、ID トークン、およびリフレッシュ・トークンと交換に、クライアントから許可付与 (許可コード) を受け入れる、OP 上のリソース。

### Open ID Connect の実装（Node.js 編）

[Node\.js で OpenID Connect の OP と RP を実装してみた \- Qiita](https://qiita.com/moomooya/items/97864e1078a3cc204c17)

[oidc\-provider \- npm](https://www.npmjs.com/package/oidc-provider)

[auth0\-js \- npm](https://www.npmjs.com/package/auth0-js)

## 03. OAuth 2.0 とは

RFC が策定した、3rd パーティに限定的なリソースアクセスを可能にする認可フレームワーク

[OAuth 2\.0 全フローの図解と動画 \- Qiita](https://qiita.com/TakahikoKawasaki/items/200951e5b5929f840a1f)

> RFC 6749(The OAuth 2.0 Authorization Framework)で定義されている 4 つの認可フロー、および、リフレッシュトークンを\
> 用いてアクセストークンの再発行を受けるフローの図解及び動画です。

1. 認可コードフロー ... 認可エンドポイントに認可リクエストを投げ、応答として短命の認可コードを受け取り、その認可コードをトークンエンドポイントでアクセストークンと交換するフロー
1. インプリシットフロー ... 認可エンドポイントに認可リクエストを投げ、応答として直接アクセストークンを受け取るフロー
1. リソースオーナー・パスワード・クレデンシャルズフロー ... トークンエンドポイントにトークンリクエストを投げ、応答としてアクセストークンを受け取るフロー。OAuth のフローだが、クライアントアプリケーションがユーザーの ID とパスワードを受け取る
1. クライアント・クレデンシャルズフロー ... トークンエンドポイントにトークンリクエストを投げ、応答としてアクセストークンを受け取るフロー。ユーザーの認証はおこなわれず、クライアントアプリケーションの認証のみがおこなわれる
1. リフレッシュトークンフロー ... 事前に発行を受けていたリフレッシュトークンをトークンエンドポイントに提示することにより、アクセストークンの再発行を受ける

### 許可コード・フロー

OpenID Connect の標準的なフロー

![openid_flow.png](https://imgur.com/b7yjStb.png)

### 暗黙的フロー

暗黙的フローは、OpenID Connect プロバイダーとして機能している Liberty サーバーでのみサポートされます。

![openid_flow2.png](https://imgur.com/OkEytDd.png)

### 『OAuth 2.0 をはじめよう』を読む

OpenID Certified されている、主な Web サービス

[OpenID Certification – OpenID](https://openid.net/certification/)

- Auth0 - Auth0 ... Basic OP, Implicit OP, Hybrid OP, Config OP
- Authelete - Authlete 1.1 ... Basic OP, Implicit OP, Hybrid OP, Config OP
- Google - Google Federated Identity ... Basic OP, Implicit OP, Hybrid OP, Config OP
- LINE - LINE Login ... Basic OP
- Microsoft - ADFS on Windows Server 2016 ... Basic OP, Implicit OP, Config OP
- Microsoft - Azure Active Directory ... Config OP
- NEC - NC7000-3A-OC ... Basic OP
- PayPal - Login with PayPal ... Config OP
- Recruit - Recruit ID ... Basic OP
- Yahoo! Japan - Yahoo! ID Federation v2 ... Basic OP, Implicit OP, Hybrid OP, Config OP

[OpenID Connect 対応してる Web サービス/製品のログイン認証関連のドキュメントリンク集 \- Qiita](https://qiita.com/minamijoyo/items/62c90abd7785820705eb)

- [Google Identity Platform](https://developers.google.com/identity/protocols/OpenIDConnect)
- [LINE ログイン](https://developers.line.me/ja/docs/line-login/overview/)
- [Yahoo! ID 連携](https://developer.yahoo.co.jp/yconnect/)
- [Yahoo! JAPAN の OpenID Certified Mark 取得について](https://www.slideshare.net/kura_lab/yahoo-japanopenid-certified-mark)

## 04. SAML とは

SAML = Security Assertion Markup Language\
ユーザーの認証や属性、認可に関する情報を記述するマークアップ言語\
SAML を使えば、クッキーを利用せずに認証情報の伝達が可能

### 参考記事 1 を読む

[SAML とは ｜クラウド型シングルサインオン・アクセスコントロール（IDaaS） OneLogin \- サイバネット](https://www.cybernet.co.jp/onelogin/function/saml.html)

> OASIS によって策定された、ユーザー認証のための規格\
> SAML を利用することで、ひとつのディレクトリで、複数のサービスへのログインへのシングルサインオンを\
> 実現する。また、ユーザーの属性情報なども付与することが可能

- Authentication ... 本人認証
- Authorization ... 利用認可

> 認証情報を提供する側 => IdP (Identity Provider)
> 認証情報を利用する側 => SP (Service Provider)

### 参考記事 2 を読む

[SAML 認証はどのように機能するか？](https://auth0.com/blog/jp-how-saml-authentication-works/)

SAML は標準 シングル サインオン(SSO)形式です。認証情報はデジタル署名した XML 文書を通じて交換します。

- Identity Provider (Auth0)
- Service Provider (Zagadat と呼ばれる企業 HR ポータル)

アプリケーションでの SAML 認証のプロセスフローをよく見てみましょう

![saml](https://imgur.com/btmVYIr.png)

### OpenID と SAML の違い

[やさしい言葉で理解する SAML 認証のまとめ \- Qiita](https://qiita.com/pasta_kun/items/4a57bd3b71ac8bf5d736)

> - OpenID とくらべて SAML の方が連携するための敷居が高い
> - SAML ではそもそも連携するかどうかユーザの選択権がない
> - SAML ではユーザが実際にログイン情報を渡すサービスを選べない
>
> SAML 認証には強制力があるため "すべてのユーザに利用させたい" 場合に向いている\
> そのため企業や団体など組織単位で利用するサービスに使われるケースが多い
