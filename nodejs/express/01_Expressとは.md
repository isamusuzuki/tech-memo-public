# Express

作成日 2021/01/17

## 01. Express とは

Node.js 環境におけるデフォルトの Web サーバー

公式サイト => [https://expressjs.com/](https://expressjs.com/)

最新バージョンは `v4.17.1`

インストール => `npm install --save express`

## 02. Express を使う

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

tsc コマンドでコンパイルしてから使う

```bash
npx tsc

node main.js
```
