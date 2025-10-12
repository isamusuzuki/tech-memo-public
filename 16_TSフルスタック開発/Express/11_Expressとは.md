# Express

作成日 2021/01/17、更新日 2025/10/12

## 1. Expressとは

Node.js 環境におけるデフォルトの Web サーバー

公式サイト => [https://expressjs.com/](https://expressjs.com/)

インストール => `npm i express`

## 2. Expressを使う

src/server.ts

```javascript
import express from 'express'
import path from 'path'

const app = express()

app.use(express.static(path.join(__dirname, 'static')))

app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'templates/index.html'))
})

app.get('*', (req, res) => {
  res.status(404).send('404 Not Found')
})

app.listen(3000, () => {
  console.log('🚀 Server ready at: http://localhost:3000')
})
```

tsc コマンドでコンパイルしてから使う

```bash
npx tsc

node dist/server.js
```
