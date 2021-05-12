# Morgan を使う

作成日 2021/01/17

HTTP リクエストロガー

公式サイト => [https://github.com/expressjs/morgan](https://github.com/expressjs/morgan)

インストール => `npm install --save morgan`

src/main.ts

```js
import express from 'express'
import morgan from 'morgan'

const app: express.Express = express()
app.use(morgan('combined'))
```

## あらかじめ決められたフォーマットの種類

- combined ... Standard Apache combined log output
- common   ... Standard Apache common log output
- dev      ... Concise output colored by response status for development use
- short    ... Shorter than default
- tiny     ... The minimal output
