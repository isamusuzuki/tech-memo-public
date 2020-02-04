---
tags: firebase
---

# Cloud Firestoreリアルタイムアップデート

作成日 2019/10/30

## 谷田部さんの説明

FirestoreのJSライブラリを使ったら、ホームページで残件数を出しているところが、
サーバーの値をいじるだけで、すぐに変わるようになった

Websocketなどの技術を使って、即時更新を実現している

[Cloud Firestore でリアルタイム アップデートを入手する](https://firebase.google.com/docs/firestore/query-data/listen?hl=ja)

## 谷田部さんが作成したダッシュボード

サイト => [https://eg-dashboads.firebaseapp.com/](https://eg-dashboads.firebaseapp.com/)

リポジトリ => [https://github.com/everglowtrading/egwms-dashboard](https://github.com/everglowtrading/egwms-dashboard)

チュートリアル => [Cloud Firestore Web Codelab](https://codelabs.developers.google.com/codelabs/firestore-web/index.html)

### index.htmlが読み込んでいるJSファイル群

```htmlmixed=
<!-- update the version number as needed -->
<script defer src="/__/firebase/7.2.0/firebase-app.js"></script>
<!-- include only the Firebase features as you need -->
<script defer src="/__/firebase/7.2.0/firebase-auth.js"></script>
<script defer src="/__/firebase/7.2.0/firebase-firestore.js"></script>
<!-- initialize the SDK after all desired features are loaded -->
<script defer src="/__/firebase/init.js"></script>

<script defer src="/scripts/main.js"></script>
```

main.jsの概要

```javascript=
var firestore = firebase.firestore();

var query = firebase.firestore().collection('egwms').doc('shipping-today-counts');

query.onSnapshot({
  includeMetadataChanges: true
}, function(change){})
```

### 似たようなチュートリアルを使っている記事を発見

[Firebase authenticationでサインアップ / ログイン機能の実装 \- Qiita](https://qiita.com/Oliverteru/items/08bf29424f691bf87e4d)

チュートリアル => [Firebase Web Codelab](https://codelabs.developers.google.com/codelabs/firebase-web/index.html#0)
