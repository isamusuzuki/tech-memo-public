# AMPまとめ

作成日 2018/04/04

## AMPとは

モバイルページの高速化プロジェクト

[AMP（Accelerated Mobile Pages）の基礎知識から対応・導入方法まで！まとめ \- ウェブ企画ラボ](https://webkikaku.co.jp/blog/seo/accelerated-mobile-pages/)

[Google ウェブマスター向け公式ブログ: Accelerated Mobile Pages プロジェクトについて \-\- 導入ガイド日本語版を本日公開しました](https://webmaster-ja.googleblog.com/2016/01/accelerated-mobile-pages.html)

## [Google導入ガイド日本語版PDF](https://drive.google.com/file/d/0BxvWUiBQ8jznNXhISW4zRFF0eW8/view)まとめ

AMPバージョンを用意すべきページ => 個々の記事ページやブログ投稿などに最適で、メインページ、カテゴリ一覧、検索ページには最適でない

AMPバージョンの記事は元のバージョンとまったく同じように見える必要がある

```html
<!-- AMP HTMLバージョンから正規バージョンにリンクさせる -->
<linkrel="canonical" href="http://example.ampproject.org/article.html"/>

 <!-- 正規バージョンから対応するAMP HTMLバージョンにリンクさせる -->
 <linkrel="amphtml" href="http://example.ampproject.org/article.amp.html"/>
```

## AMPページを作成する

[Accelerated Mobile Pages Project – AMP](https://www.ampproject.org/ja/)

[はじめに – AMP](https://www.ampproject.org/ja/docs/getting_started/quickstart)

```html
<!doctype html>
<html ⚡>
  <head>
    <meta charset="utf-8">
    <script async src="https://cdn.ampproject.org/v0.js"></script>
    <title>Hello AMP world</title>
    <link rel="canonical" href="hello-world.html">
    <meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
    <style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
  </head>
  <body>
    <h1>Hello AMP World!</h1>
  </body>
</html>
```

画像を追加するときは、amp-img要素を使う

```html
<amp-img src="https://www.ampproject.org/examples/images/amp.jpg"
  width="900" height="508" layout="responsive"></amp-img>
```

独自のスタイルを設定するときは、style要素にamp-customを追加する

```html
<style amp-custom>
  amp-img {
    margin: 0.5em;
  }
  body {
    max-width: 900px;
  }
</style>
```

## Nuxt.jsでAMPを作成するには

[nuxt\.js/examples/with\-amp at dev · nuxt/nuxt\.js](https://github.com/nuxt/nuxt.js/tree/dev/examples/with-amp)

[vue\.js \- Google AMP with nuxt\.js \- Stack Overflow](https://stackoverflow.com/questions/49032138/google-amp-with-nuxt-js)

[AMP example doesn't work · Issue \#2661 · nuxt/nuxt\.js](https://github.com/nuxt/nuxt.js/issues/2661)

## AMP Story

GoogleがAMP Storyという新しいフォーマットを発表した。この新しいフォーマットを利用すれば、サイトのパブリッシャーはモバイル向けに画像、ビデオ、アニメーションを多量に含んだページを提供してもユーザーが楽にナビゲーションできるようになる。

[Stories – AMP](https://www.ampproject.org/stories)

## AMP for Email

GoogleはAMPをメールに適用した。

[Bringing the power of AMP to Gmail](https://www.blog.google/products/g-suite/bringing-power-amp-gmail/)

[Googleが推進するウェブ高速化フレームワークのAMPが電子メールに対応、2018年後半にGmailにも導入へ \- GIGAZINE](https://gigazine.net/news/20180214-google-amp-gmail/)

> 実際に「AMP for Email」の誕生で、電子メール上でさまざまなタスクが完了可能になります。一体どういうことかというと、例えば複数人に向けて何かしらのイベントの出欠確認を行う際、通常は出欠管理ツールを用いて出欠確認を行います。この場合、幹事役の人はわざわざメールで候補者にURLを送信し、候補者側はURLをクリックしてウェブページへ飛び、そこで出欠情報を入力する必要があります。しかし「AMP for Email」を使えば、わざわざ本文中のURLを開かなくても、メール上で出欠情報の入力を済ませることが可能です。

## [AMP Start](https://www.ampstart.com/)とは

[リアルガチにヤバいAMP Start \- Qiita](http://qiita.com/tsushimori/items/20b7478e9fb86d2acbfd)

> AMP Startはただのコンポーネントフレームワークではない。サイト全体がAMP化できるということを示唆している... これを使えばウェブサイト自体もAMP化できる。
>
> デザイン整っててヤバい
>
> メニューやボタンが追加されたことで広がる世界
>
> このAMP Startではサンプルにスマホ、タブレット、PCと３つの確認ができるようになっています。これが、ウェブサイト適用への入り口になっていると感じた点です。
>
> そんなときにAMP Startとかを念頭に設計していけば、モバイルがとにかくきれいであり、表示も早いということで、設計のベクトルを変えていくきっかけになっていくのではないかと期待しております。
