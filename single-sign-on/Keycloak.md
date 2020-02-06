---
tags: sso
---

# Keycloak

作成日 2019/11/13、更新日 2019/11/14

## 01. Keycloakとは

公式トップ => [https://www.keycloak.org/](https://www.keycloak.org/)

日本語ドキュメント => [https://keycloak-documentation.openstandia.jp/](https://keycloak-documentation.openstandia.jp/)

- [Getting Started](https://keycloak-documentation.openstandia.jp/master/ja_JP/getting_started/index.html)
- [Server Installation](https://keycloak-documentation.openstandia.jp/master/ja_JP/server_installation/index.html)
- [Securing Apps](https://keycloak-documentation.openstandia.jp/master/ja_JP/securing_apps/index.html)
- [Server Admin](https://keycloak-documentation.openstandia.jp/master/ja_JP/server_admin/index.html)
- [Server Development](https://keycloak-documentation.openstandia.jp/master/ja_JP/server_development/index.html)
- [Authorization Services](https://keycloak-documentation.openstandia.jp/master/ja_JP/authorization_services/index.html)
- [Upgrading](https://keycloak-documentation.openstandia.jp/master/ja_JP/upgrading/index.html)

## 02. 参考記事を読む

[Keycloakとは \- Qiita](https://qiita.com/daian183/items/30f01e162e03567ff21b)

>Keycloakはオープンソースのアイデンティティ・アクセス管理ソフトウェア\
>シングルサインオンやAPIアクセスの認証・認可制御を実現する

[マイクロサービス時代のSSOを実現する「Keycloak」とは](http://www.atmarkit.co.jp/ait/articles/1708/31/news011.html)

>OAuthは、Webアプリケーションによる認可を行うためのオープンなプロトコル標準\
>OIDCは、OAuthによる認可に加え、JWTベースの「IDトークン」を利用する\
>IDトークンの発行を行う認可プロバイダーの構築、\
>APIサーバーによるIDトークンを検証する機能の追加\
>=> パッケージソフトの利用からOSS実装へ

[KeycloakでSAMLを使ってみる（WordPress編） \- Qiita](https://qiita.com/katakura__pro/items/1e65e0bde7fda75332a1)

### Keycloakの機能

- 認証：自分での認証
- 認証：ディレクトリサーバ連携
- 認証：外部IdP連携
- 認可：認可プロバイダー（OAuth, OIDC, SAML対応）
- 認可：クライアントプロキシ（リバースプロキシとして動作）
- 認可：クライアントアダプタ（ライブラリ提供）

## 03. 「Keycloakのセットアップ」を読む

[Keycloakのセットアップ \- Qiita](https://qiita.com/tamura__246/items/13fc301e9409fef77bf3)

### Keycloakを起動するための要件

Javaを実行可能なOS, Java 8 JDK, RAM512MB以上, 1GB以上のディスクスペース

### Keycloakインストールの準備

CentOS7.5にJava 8をインストールする

```bash=
yum search java-1.8.0-openjdk

sudo yum install java-1.8.0-openjdk
sudo yum install java-1.8.0-openjdk-devel

java -version
# => openjdk version "1.8.0_111"

which java
# => /usr/lib/jrm/java
```

Keycloak公式サイトから、Keycloakの圧縮ファイルをダウンロード\
適当なディレクトリで解凍して、スクリプトを実行する\
Keycloakは、Wildfly(JavaEEコンテナ)が同梱されており、この上で動作する

```bash=
unzip keycloak-3.3.0.CR2.zip

./bin/standalone.sh
# サーバーを停止するには、Ctrl+C
```

### 管理者ユーザーの作成

`http://localhost:8080/auth`にアクセスするか、`add-user-keycloak.sh`で作成可能

### 管理コンソールへのログイン

ウェルカムページからAdministration Consoleのリンクをクリック

### 日本語化

左枠 ＞ Realm Settings ＞ 右枠 ＞ Themesタブ ＞ Internationalization Enabledをオンにする ＞ Saveボタン

