# moment

作成日 2021/01/26

公式サイト => [http://momentjs.com/](http://momentjs.com/)

インストール => `npm install --save moment`

```javascript
const moment = require('moment');
moment.locale('ja');

/* 基本的な使い方 */
moment().format('YYYY-MM-DD HH:mm:ss');
//=> 2017-05-10 10:30:58
moment().format('YYYYMMDD_HHmmss');
//=> 20170510_103058
moment().format('LLdddd');
//=> 2017年5月10日水曜日

/* 指定した分だけ時間を進めることができる */
let thisDate = moment('20170630');
thisDate.format('LLdddd');
//=> 2017年6月30日金曜日
thisDate.add(27, 'hours');
thisDate.format('YYYY-MM-DD HH:mm:ss');
//=> 2017-07-01 03:00:00

/* 期間を調べることもできる */
let startDate = moment('20160406');
let endDate = moment('20160518');
endDate.diff(startDate, 'days');
//=> 42
```
