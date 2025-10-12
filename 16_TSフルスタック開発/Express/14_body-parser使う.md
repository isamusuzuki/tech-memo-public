# body-parserを使う

作成日 2021/01/17

POST受信で取得した`application/x-www-form-urlencoded`形式のデータをJSON形式のデータに変換する

公式サイト => [https://github.com/expressjs/body-parser](https://github.com/expressjs/body-parser)

インストール => `npm i body-parser`

src/server.ts

```javascript
import express from 'express'
import bodyParser from 'body-parser'

const app = express()
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

app.post('/submit', (req, res) => {
  res.setHeader('Content-Type', 'text/plain; charset=utf-8')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```
