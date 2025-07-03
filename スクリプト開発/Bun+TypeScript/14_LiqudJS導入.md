# LiquidJS導入

作成日 20205/05/28

公式サイト（英語） => [LiquidJS](https://liquidjs.com/)

テキストファイルの中に埋め込んだ変数を置換するだけのテンプレートエンジンを探していた

HTMLファイルやWEBサーバーを前提とされるとかえって使いにくいという縛りがあった

サンプルコードがコンソールに出力するものだったので、LiquidJSを導入してみた

インストール => `npm install --save liquidjs`

TypeScriptのサンプルコード

```javascript
import { Liquid } from 'liquidjs';
const engine = new Liquid();

engine
    .parseAndRender('{{name | capitalize}}', {name: 'alice'})
    .then(console.log);     // outputs 'Alice'
```

第一引数と第二引数をそれぞれファイルから読み込んだ文字列にしても問題なく動作した

```javascript
(async() => {
    // テンプレファイルを読み込む
    const tmplPath = path.join(__dirname, '../temp/01.sql.tmpl');
    const tmpl = fs.readFileSync(tmplPath, 'utf8');
    // JSONファイルを読み込む
    const jsonPath = path.join(__dirname, '../temp/returning.json');
    const jsonData = fs.readFileSync(jsonPath, 'utf8');
    const returning = JSON.parse(jsonData);

    const engine = new Liquid();
    engine.parseAndRender(tmpl, returning)
        .then(result => {
            // SQL文を出力する
            const sqlPath = path.join(__dirname, '../temp/01.sql');
            fs.writeFileSync(sqlPath, format(result), 'utf8');
            console.log(`SQL file created at ${sqlPath}`);            
        });
})();
```
