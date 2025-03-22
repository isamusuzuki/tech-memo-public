# commander

作成日 2021/03/22

Node.js で、CLI プログラムを完成させるための便利ツール

公式 => [https://github.com/tj/commander.js](https://github.com/tj/commander.js)

インストール => `npm install --save commander`

## commander の使い方

```bash
node amazon.js -a B01N7F58X
node amazon.js --asin B01N7F58X

node amazon.js
# => error: required option '-a, --asin [asin]' not specified
```

amazon.js

```javascript
const { program } = require('commander');

program.requiredOption(
    '-a, --asin [asin]', 'ASIN'
).parse(process.argv);
const options = program.opts();

if (!/^[0-9A-Z]{10}$/.test(options.asin)) {
  throw Error('正しいASINではありません');
} else {
  fetchAmazon(options.asin);
}
```
