# JSON Web Tokens

作成日 2021/01/26

公式サイト => [http://jwt.io/](http://jwt.io/)

インストール => `npm install --save jsonwebtoken`

- トークンは 3 つの文字列をピリオドで繋いだ形として登場する
- `<header>.<payload>.<signature>`
- 公式ページのオンラインデバッガーでも、payload の内容は取り出すことができる
- しかし signature はわからなのいで、invalid signature となる
- payload に大事なデータを入れてはいけない。それだけ守れば JWT は安全である

```javascript
const jwt = require('jsonwebtoken');
const superSecret = 'SUPER SECRET';

// Sign
let token = jwt.sign({ id: id }, superSecret, { expiresIn: '1d' });
// expiresInで指定できるもの
// 数字を入れたら秒扱い 60 => 60 seconds
// 期間を表す文字列 '2 days', '10h', '7d'

// Verify
jwt.verify(data, superSecret, function (err, decoded) {
  if (err) throw err;
  console.log('Success! data = ' + JSON.stringify(decoded));
});
```
