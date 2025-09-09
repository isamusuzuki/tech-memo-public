# uuid

作成日 2021/01/26

RFC4122 の UUID（汎用一意識別子）を作成する

インストール => `npm install --save uuid`

```javascript
const uuidv1 = require('uuid/v1');
uuidv1();
//=> Version 1 (timestamp)

const uuidv4 = require('uuid/v4');
uuidv4();
//=> Version 4 (random)
```
