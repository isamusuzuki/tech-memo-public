# Handlebarsをテンプレートエンジンとして使う

作成日 2021/01/24

## 1. Handlebarsとは

Express用のテンプレートエンジン

公式サイト => [https://github.com/express-handlebars/express-handlebars](https://github.com/express-handlebars/express-handlebars)

インストール => `npm i express-handlebars`

## 2. Expressサーバーに組み込む

src/server.ts

```javascript
import express from 'express'
import handlebars from 'express-handlebars'

const app = express()

app.engine('.hbs', handlebars({ extname: '.hbs', defaultLayout: 'main' }))
app.set('view engine', '.hbs');

app.get('/', (req, res) => {
  res.render('home')
})

app.listen(8080, () => {
  console.log('🚀 Server ready at: http://localhost:3000')
})
```

## 3. テンプレートファイルの書き方

layoutsDir のデフォルトは `views/layouts/` となっている

```text
--PROJECT-FOLDER/
    |--views/
    |   |--layouts/
    |   |   `--main.hbs
    |   `--home.hbs
    `--main.js
```

views/layouts/main.hbs

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Sandbox</title>
  </head>
  <body>
    <h1>Sandbox</h1>
    {{{ body }}}
  </body>
</html>
```

views/home.hbs

```html
<h2>home</h2>
```

=> 出来上がり

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Sandbox</title>
  </head>
  <body>
    <h1>Sandbox</h1>
    <h2>home</h2>
  </body>
</html>
```

## 4. partials機能を使う

```text
--PROJECT-FOLDER/
    |--views/
    |   |--layouts/
    |   |   `--main.hbs
    |   |--partials/
    |   |   `--list.hbs
    |   `--home.hbs
    `--main.js
```

views/layouts/main.hbs

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Sandbox</title>
  </head>
  <body>
    <h1>Sandbox</h1>
    {{{ body }}}
  </body>
</html>
```

view/home.hbs

```html
<h2>home</h2>
{{> list num="123"}}
{{> list num="456"}}
```

list.hbs

```html
<ul>
  <li>abc {{num}}</li>
  <li>def {{num}}</li>
</ul>
```

=> 出来上がり

```html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <title>Sandbox</title>
  </head>
  <body>
    <h1>Sandbox</h1>
    <h2>home</h2>
    <ul>
        <li>abc 123</li>
        <li>def 123</li>
    </ul>
    <ul>
        <li>abc 456</li>
        <li>def 456</li>
    </ul>
  </body>
</html>
```
