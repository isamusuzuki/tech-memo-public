# Express まとめ

作成日 2021/01/17

## 01. Express とは

Node.js 環境におけるデフォルトの Web サーバー

公式サイト => [Express \- Node\.js web application framework](https://expressjs.com/)

最新バージョンは `v4.17.1`

## 02. Express を使う

インストール => `npm install --save express`

src/main.ts

```javascript
import express from 'express'
import path from 'path'

const app: express.Express = express()

app.use(express.static(path.join(__dirname, 'static')))

app.get('/', (req: express.Request, res: express.Response) => {
    res.sendFile(path.join(__dirname, 'templates/index.html'))
})

app.get('*', (req: express.Request, res: express.Response) => {
    res.status(404).send('404 Not Found')
})

app.listen(8080, () => {
    console.log('app listening on port 8080.')
})
```

tscコマンドでコンパイルしてから使う

```bash
npx tsc

node main.js
```

## 03. Request オブジェクトを使いこなす

[http://expressjs.com/en/api.html#req](http://expressjs.com/en/api.html#req)

Request オブジェクトを使った、様々な情報を収集する

```javascript
router.get('/', function (req, res) {
  var obj = {
    hostname: req.hostname,
    ip: req.ip,
    originalUrl: req.originalUrl,
    params: req.params,
    path: req.path,
    protocol: req.protocol,
    query: req.query,
  }
  res.send(JSON.stringify(obj))
})
```

返事の例

```json
{
    "hostname":"localhost",
    "ip":"::1",
    "originalUrl":"/avocado",
    "params":{},
    "path":"/",
    "protocol":"http",
    "query":{}
}
```

## 04. Response オブジェクトを使いこなす

[http://expressjs.com/en/api.html#res](http://expressjs.com/en/api.html#res)

Reponse オブジェクトを使って返事をする

```javascript
res.send('Hello, World!')
res.status(404).send('404 Not Found')

res.sendFile(path.join(__dirname, 'templates/index.html'))

res.setHeader('Content-Type', 'text/plain; charset=utf-8')
res.write('受信したデータは次の通り\n')
res.write(data)
res.end('以上')

res.json({ user: 'tobi' })
res.status(500).json({ error: 'message' })

res.redirect(301, 'http://example.com')

res.render('home')
```

## 05. body-parser を使いこなす

POST 受信で取得した `application/x-www-form-urlencoded` 形式のデータを JSON 形式のデータに変換する

公式サイト => [expressjs/body\-parser: Node\.js body parsing middleware](https://github.com/expressjs/body-parser)

インストール => `npm install --save body-parser`

src/main.ts

```javascript
import express from 'express'
import bodyParser from 'body-parser'

const app: express.Express = express()
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

app.post('/submit', (req: express.Request, res: express.Response) => {
    res.setHeader('Content-Type', 'text/plain; charset=utf-8')
    res.write('you posted:\n')
    res.end(JSON.stringify(req.body, null, 2))
})
```

## 06. Multer を使いこなす

POST 受信で取得した `application/x-www-form-urlencoded` 形式のデータを扱う

公式サイト => [expressjs/multer: Node\.js middleware for handling \`multipart/form\-data\`\.](https://github.com/expressjs/multer)

インストール => `npm install --save multer`

src/main.ts

```javascript
import express from 'express'
import multer from 'multer'

const app: express.Express = express()
const upload: multer.Multer = multer({ dest: 'temp/' })  // ルートからの相対パス

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

### Multer ファイルオブジェクト

下記のフィールドを持つ JSON オブジェクトが request オブジェクトに渡る

`req.file` でその JSON オブジェクトが読める

- fieldname    ... HTML フォームで指定された名前
- originalname ... アップロードしたファイルのファイル名
- name         ... 勝手に変更されたファイル名
- encoding     ... ファイルのエンコードタイプ
- mimetype     ... ファイルの MIME タイプ
- path         ... サーバー側に保存されたファイルのパス
- extension    ... ファイルの拡張子
- size         ... ファイルサイズ(byte)
- truncated    ... サイズ制限によってファイルが切られたかどうか
- buffer       ... inMemory オプションを true にした場合に、ファイルの生データ

## 07. Morgan を使いこなす

HTTP リクエストロガー

公式サイト => [expressjs/morgan: HTTP request logger middleware for node\.js](https://github.com/expressjs/morgan)

インストール => `npm install --save morgan`

src/main.ts

```js
import express from 'express'
import morgan from 'morgan'

const app: express.Express = express()
app.use(morgan('combined'))
```

### あらかじめ決められたフォーマットの種類

- combined ... Standard Apache combined log output
- common   ... Standard Apache common log output
- dev      ... Concise output colored by response status for development use
- short    ... Shorter than default
- tiny     ... The minimal output
