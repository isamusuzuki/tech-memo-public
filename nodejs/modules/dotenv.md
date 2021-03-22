# dotenv

作成日 2021/03/22

.env ファイルの中身を環境変数に組み込むツール

公式 => [https://github.com/motdotla/dotenv](https://github.com/motdotla/dotenv)

インストール => `npm install --save dotenv`

## dotenv の使い方

```bash
node sample.js
```

rms.js

```javascript
require('donenv').config();
const db = require('db');

db.connect({
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS,
});
```

.env

```text
DB_HOST=localhost
DB_USER=root
DB_PASS=123456
```
