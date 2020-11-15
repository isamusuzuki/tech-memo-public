# Bulma を使う

作成日 2020/11/15

## 01. Bulma とは

CSS ファイル 1 つで動く CSS フレームワーク

公式サイト => [Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/)

ドキュメント => [Documentation \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/)

## 02. Bulma をインストールする

CDN を使う方法が一番カンタン

[bulma CDN by jsDelivr \- A CDN for npm and GitHub](https://www.jsdelivr.com/package/npm/bulma)

現在の最新バージョンは `0.9.1`

css フォルダの中に `bulma.min.css` があるので、それを読み込む

=> `https://cdn.jsdelivr.net/npm/bulma@0.9.1/css/bulma.min.css`

### サンプルコード

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Hello Bulma!</title>
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bulma@0.9.1/css/bulma.min.css"
    />
  </head>
  <body>
    <section class="section">
      <div class="container">
        <h1 class="title">Hello World</h1>
        <p class="subtitle">My first website with <strong>Bulma</strong>!</p>
      </div>
    </section>
  </body>
</html>
```
