# dayjs

作成日 2025/10/09

## 公式サイト（英語）を読む

[Day.js](https://day.js.org/)

> Fast 2kB alternative to Moment.js with the same modern API

Moment.jsに慣れている人は、Day.jsもすぐに使えるらしい

インストール => `npm i dayjs`

国際化をきっちりサポートしている

```javascript
import * as dayjs from 'dayjs'
import 'dayjs/locale/ja';

dayjs.locale('ja') // グローバルに日本語ロケールになる
```

[dayjs/src/locale/ja.js](https://github.com/iamkun/dayjs/blob/dev/src/locale/ja.js)
