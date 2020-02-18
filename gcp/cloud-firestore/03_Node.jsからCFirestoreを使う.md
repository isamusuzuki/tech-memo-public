# Node.jsからCloud Firestoreを使う

作成日 2019/11/11


## 01. 準備作業

### ローカルから使うにはサービスアカウントキーが必要

- 新しくサービスアカウントを作成する
- サービスアカウントの権限に「Cloud Datastore ユーザー」を追加する
- キーの作成を実行する。キータイプは JSON を選ぶ
- JSON ファイルをダウンロードして、ホームフォルダに置く
- `.bashrc` ファイルに以下を書き込む（Git Bash を利用している場合）

```bash
export GOOGLE_APPLICATION_CREDENTIALS=c:\Users\<user-name>\<service-account-key>.json
```

### Node.jsライブラリのインストール

[@google\-cloud/firestore \- npm](https://www.npmjs.com/package/@google-cloud/firestore)

```bash=
npm install @google-cloud/firestore --save
```

## 02. Node.jsライブラリのドキュメント

[https://googleapis.dev/nodejs/firestore/latest/index.html](https://googleapis.dev/nodejs/firestore/latest/index.html)

## 03. Node.jsサンプルコード

### `set()`メソッドを使って、指定したドキュメントにデータを登録する

```javascript
const Firestore = require('@google-cloud/firestore');

(async () => {
    const db = new Firestore({ projectId: 'test-project' });
    const docRef = db.collection('test-collection').doc('test-doc');
    await docRef.set({
        message: 'test-test-test',
        created_at: Firestore.FieldValue.serverTimestamp(),
    });
})().catch(e => console.error(e));
```

### `where()`メソッドを使って、条件にマッチしたドキュメントにアクセスする

```javascript
const db = new Firestore({ projectId: 'test-project' });
const collRef = db.collection('test-collection');
collRef.where('willFollow', '==', true).limit(1).get()
.then(async snapshot => {
    if (snapshot.empty) {
        console.log('no mathching');
        return;
    }

    let doc = snapshot.docs[0];
    console.log(doc.id, '=>', doc.data());
})
.catch(err => {
    console.log('Error', '=>', err)
});
```

### `update()`メソッドを使って、指定したドキュメントのデータを更新する

```javascript
const db = new Firestore({ projectId: 'test-project' });
const collRef = db.collection('test-collection');
const docRef = await collRef.doc(snapshot.docs[0].data['name']);
await docRef.update({
    didFollow: true,
})
.catch(err => {
    console.log('Error', '=>', err)
});
```
