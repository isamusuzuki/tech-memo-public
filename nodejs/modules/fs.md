# fs (File System)

作成日 2021/01/26、更新日 2021/03/07

ドキュメント => [https://nodejs.org/api/fs.html](https://nodejs.org/api/fs.html)

## 01. ファイルを保存する

### 01-1. コールバックを使ってファイルを保存した場合

```javascript
const fs = require('fs');

fs.writeFileSync(`./temp/${name}.json`, JSON.stringify(result, null, 2));
```

### 01-2. プロミスを使ってファイルを保存した場合

```javascript
import { promises as fs } from 'fs'

(async () => {
  const filepath = process.argv[2]
  const result = await somethingPromise()
  await fs.writeFile(filepath, JSON.stringify({ result }, null, 2))
})().catch(e => console.error(e))
```

## 02. ファイルを読む

### 02-1. コールバックを使ってファイルを読む場合

```javascript
const fs = require('fs');

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

### 02-2. プロミスを使ってファイルを読む場合

```javascript
import { promises as fs } from 'fs'

const apple = async () => {
    await fs.readFile('data/kifu01.json', 'utf-8').then(body => {
        const body_dict = JSON.parse(body)
        const score_list = <Array<string>>body_dict['score']
        console.log(`score count => ${score_list.length}`)
        score_list.forEach(score => {
            console.log(score)            
        })
    })
}
apple()
```
