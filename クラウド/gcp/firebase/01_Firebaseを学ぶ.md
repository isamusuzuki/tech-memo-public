---
tags: firebase
---

# Firebase

作成日 2019/06/02

## 01. Firebaseとは

包括的なモバイル開発プラットフォーム\
モバイルアプリだけでなく Web アプリもこれひとつで開発可能

- Cloud Firestore ... NoSQL データベース
- Cloud Functions ... サーバーなしでバックエンドを開発
- Firebase Hosting ... CDN
- Firebase Auth ... 認証
- Cloud Storage ... ユーザーが生成したコンテンツの保存

公式トップ => [https://firebase.google.com/](https://firebase.google.com/)

### Firebaseの紹介記事を読む

[Firebase で完結するリッチな Web アプリ構築の勘所 \- Qiita](https://qiita.com/y_kawase/items/1de690b40553ef76acb3)

>Firebase を使って良かったところ
>- 認証が大変楽に、このご時世、認証周りを自分で書いてる人なんているんです？
>- Firestore はシンプルながら高機能、簡単にリアルタイムデータ同期ができます

[Firebase\+Vue\.js で大学の関係者だけが見れるサイトをサクッと作る \- Qiita](https://qiita.com/reud/items/06f2b87e96c7384e1609)

この記事は、WebStorm の画面、サイトの画面が 1 枚づつ丁寧に貼られているのでわかりやすい

## 02. Firebase Hostingとは

ドキュメント => [https://firebase.google.com/docs/hosting/?hl=ja](https://firebase.google.com/docs/hosting/?hl=ja)

- プロジェクトで `firebaseapp.com` ドメインのサブドメインを使用できる
- 独自のドメイン名を接続することもできる
- Firebase CLI を使用する

```bash=
# ローカルでサイトをテストする
firebase serve

# サーバーにサイトをアップする
firebase deploy
```

### Firebase CLIをインストールする

[Firebase CLI リファレンス  \|  Firebase](https://firebase.google.com/docs/cli?hl=ja)

Firebase CLIを使用する前に、Firebaseプロジェクトを作成しておく必要がある

[Firebase console](https://console.firebase.google.com/u/1/)

### Flamelinkとは

Firebaseに特化したヘッドレスCMS\
Built for Google's Firebase

公式トップ => [https://flamelink.io/](https://flamelink.io/)

紹介記事 => [Firebase、Flamelink、Nuxt、Netlify、PWAを使ってJAMstackなブログを作る](https://qiita.com/miyaoka/items/9774f0250953da419a58)

データはFirebaseにあるので、「コンテンツの編集管理画面」と言い切れるかもしれない
