# process

作成日 2021/01/26

ドキュメント => [Process \| Node\.js v15\.6\.0 Documentation](https://nodejs.org/api/process.html)

## 01. process.env で環境変数を取得する

```bash
NODE_ENV=development node app.js
# => console.log(process.env.NODE_ENV)
# => development
```

## 02. process.argv でコマンドライン引数を取得する

```bash
node app.js aaa bbb ccc
# => console.log(process.argv)
# => ['node', 'app.js', 'aaa', 'bbb', 'ccc']
```
