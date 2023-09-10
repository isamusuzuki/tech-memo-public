# OGPまとめ

作成日 2018/05/02

## OGP (Open Graph Protocol)とは

[The Open Graph protocol](http://ogp.me/)

SNSでシェアされた際に、そのページのタイトル・URL・概要・アイキャッチ画像（サムネイル）を
意図した通りに正しく表示させる仕組み

```html
<meta property="og:title" content="ページのタイトル">
<!-- og:title | Basic | titleタグよりも優先される -->
<meta property="og:type" content="ページの種類">
<!-- og:type | Basic | トップページならwebsite、それ以外はarticleにする -->
<meta property="og:url" content="ページのURL">
<!-- og:url | Basic | 絶対パス -->
<meta property="og:image" content="サムネイル画像のURL">
<!-- og:image | Basic | 絶対パス、facebookは1200x630pxを推奨 -->
<meta property="og:site_name" content="サイト名">
<!-- og:site_name | Optional | サイト名 -->
<meta property="og:description" content="ページのディスクリプション">
<!-- og:description | Optional | ページの説明 -->
<meta property="fb:app_id" content="App-ID（15文字の半角数字）">
<!-- fb:app_id | facebook用 | 必須のプロパティ、ないとfacebookで表示されない -->
<meta name="twitter:card" content="Twitterカードの種類"/>
<!-- twitter:card | Twitter用 | Twitterでの表示方法 -->
<!-- summary,summary_large_image,photo,gallery,app -->
<meta name="twitter:site" content="Twiiterのユーザーネーム"/>
<!-- twitter:site | Twitter用 | @username -->
<meta name="twitter:creator" content="Twiiterのユーザーネーム"/>
<!-- twitter:site | Twitter用 | @username -->
```

## 本当に必要なメタタグはいくつなのか

一番シンプルな例

```html
<meta property="og:title" content="ページのタイトル" />
<meta property="og:description" content="ページのディスクリプション" />
<meta property="og:url" content="ページのURL">
<meta name="twitter:card" content="Twitterカードの種類"/>
<meta property="og:image" content="サムネイル画像のURL">
```

[OGPで本当に必要なメタタグは? \- Qiita](https://qiita.com/sutara79/items/d7a45f6c4796c1ee1590)
