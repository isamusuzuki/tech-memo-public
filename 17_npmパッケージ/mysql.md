# mysql

作成日 2021/01/26

インストール => `npm install --save mysql`

## 01. 基本形

```javascript
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: 'locahost',
  user: 'me',
  password: 'secret',
  database: 'my_db',
});
connection.connect();
connection.query('SELECT * FROM test1', function (err, results, fields) {
  // 取得後の動作
});
connection.end();
```

## 02. プレースホルダを使う

```javascript
// 挿入クエリー
connection.query(
  'INSERT INTO test1 SET ?',
  { username: 'taro', fullname: '岡本太郎' },
  function (err, results, fields) {
    // 取得後の動作
  }
);

// 更新クエリー
connection.query(
  'UPDATE test1 SET ? WHERE ?',
  [{ username: 'taro', fullname: '岡本太郎' }, { id: 4 }],
  function (err, result) {
    // 取得後の動作
  }
);
```

- クエスチョンマーク 1 個に、JS オブジェクトが 1 個対応する
- JS オブジェクトの中にはいくつでもプロパティが持てる
- おそらくプロパティ同士は AND で連結される（OR で連結するときは不明）
- クエスチョンマークが 2 個以上ある場合は、配列を用意する
