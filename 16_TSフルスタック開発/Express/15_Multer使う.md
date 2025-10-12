# Multerを使う

作成日 2021/01/17

POST受信で取得した`application/x-www-form-urlencoded`形式のデータを扱う

公式サイト => [https://github.com/expressjs/multer](https://github.com/expressjs/multer)

インストール => `npm i multer`

src/server.ts

```javascript
import express from 'express'
import multer from 'multer'

const app = express()
const upload = multer({ dest: 'temp/' })  // ルートからの相対パス

app.post('/upload', upload.single('file1'), function(req, res) {
  // ファイルを添付しないで送信してきたらエラー
  if(!req.file) {
    res.redirect('/error')
  }
  // アップロードしたファイルの情報を取得する
  const tmp_path = req.file.path
  const tmp_mime = req.file.mimetype
}
```

## Multerファイルオブジェクト

下記のフィールドを持つ JSON オブジェクトが request オブジェクトに渡る

`req.file` でその JSON オブジェクトが読める

- fieldname     ... HTML フォームで指定された名前
- originalname  ... アップロードしたファイルのファイル名
- name          ... 勝手に変更されたファイル名
- encoding      ... ファイルのエンコードタイプ
- mimetype      ... ファイルの MIME タイプ
- path          ... サーバー側に保存されたファイルのパス
- extension     ... ファイルの拡張子
- size          ... ファイルサイズ(byte)
- truncated     ... サイズ制限によってファイルが切られたかどうか
- buffer        ... inMemory オプションを true にした場合に、ファイルの生データ
