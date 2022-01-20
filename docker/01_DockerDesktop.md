# Docker Desktop

作成日 2022/01/20

## 01. Docker Desktop を Windows 10 にインストールする

公式サイト => [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

ドキュメント => [https://hub.docker.com/editions/community/docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

## 02. トラブル発生

Docker Desktop をインストールしたユーザーではない別のユーザーで同じ PC にログインし、インストール済みの Docker
Desktop を起動したら以下のエラーが表示された

> You are not allowed to use Docker\
> You must be in the "docker-users" group

### 解決方法

- スタートを右クリック ＞ コンピュータの管理 ＞「コンピュータの管理」ウィンドウ
- 左枠 ＞ システムツール ＞ ローカルユーザーとグループ ＞ グループ
- 右枠 ＞ docker-users をダブルクリック ＞「docker-users のプロパティ」ウィンドウ
- 選択するオブジェクト名を入力してください ＞ ユーザー名を入力 ＞ 「名前の確認」ボタン
- OK ボタン ＞ 適用ボタン ＞ OK ボタン
- PCを再起動する

## 03. Docker Desktop WSL 2 バックエンドとは

インストールされているならば、Docker DesktopがWSL2を利用して、Linuxコンテナをネイティブに実行するようになる

WSL 2 がインストールされているPCでは、Docker Desktopをインストールした時に、自動的に「有効化」されている

ドキュメント => [https://docs.docker.jp/docker-for-windows/wsl.html](https://docs.docker.jp/docker-for-windows/wsl.html)
