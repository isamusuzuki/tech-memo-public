# fs (file system)

作成日 2021/01/26

ドキュメント => [File system \| Node\.js v15\.6\.0 Documentation](https://nodejs.org/api/fs.html)

## 01. ファイルを保存する

```javascript
const fs = require('fs');
fs.writeFileSync(`./temp/${name}.json`, JSON.stringify(result, null, 2));
```

## 02. ファイルを読む

```javascript
// encodingを指定しないとraw bufferが戻る
fs.readFile('/etc/passwd', callback);

// encodingを指定すれば文字列が戻る
fs.readFile('/etc/passwd', 'utf8', callback);

// コールバック関数の第一引数はエラー、データは第二引数に入っている
fs.readFile('./temp/mail.txt', 'utf8', (err, data) => {
  if (err) console.error(err);
  console.log(data);
});
```
