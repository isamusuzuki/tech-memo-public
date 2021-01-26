# path

作成日 2021/01/26

```javascript
const path = require('path');

// path.join ディレクトリ区切り文字で文字列を結合する
const relpath = path.join('./hoge', 'fuga');
// => ./hoge/fuga

// path.resolve 相対パスを絶対パスに変換する
const abspath = path.resolve(relpath);
// => /Users/home/someone/hoge/fuga

// path.resovlveに複数個引数与えても問題ない
const abspath = path.resolve('hoge', 'fuga');
// => /Users/home/someone/hoge/fuga
```
