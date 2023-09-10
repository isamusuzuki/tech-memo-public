# HTML まとめ

作成日 2018/02/28、最終更新日 2018/05/02

## HTML テンプレート

```html
<!DOCTYPE html>
<html>
  <head lang="ja">
    <meta charset="utf-8" />
    <title>Sandbox</title>
  </head>
  <body>
    <main></main>
  </body>
</html>
```

## iPhone 機種別のブラウザサイズ

| 機種          | ポイント | Retina | ピクセル  |
| ------------- | :------: | :----: | :-------: |
| iPhone SE     | 320x568  |   2    | 640x1136  |
| iPhone 8      | 375x667  |   2    | 750x1334  |
| iPhone 8 Plus | 414x736  |   3    | 1242x2208 |
| iPhone X      | 375x812  |   3    | 1125x2436 |

## viewport の設定方法

```html
<!-- コンテンツ幅をスマホの画面幅に指定する -->
<meta name="viewport" content="width=device-width" />

<!-- コンテンツ幅をきっちり指定する -->
<meta name="viewport" content="width=640" />

<!-- initial-scaleプロパティは、ページがロードされたときのズームレベルを指定する -->
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

maximum-scale,minimum-scale,user-scalable プロパティは、ユーザーにズームを
許可するのか、しないのか、制限付きで許可するのかを指定する

`user-scalable=1.0 or yes`は、ユーザーにズームを許可する

`user-scalable=no`は、ユーザーにズームを許可しない（yes か no のみ）

## Web からのカメラアクセスについて

[ユーザーから画像を取得する  \|  Web  \|  Google Developers](https://developers.google.com/web/fundamentals/media/capturing-images/?hl=ja)

```html
<input type="file" accept="image/*" capture="camera" id="camera" />
<img id="frame" />
<script>
  var camera = document.getElementById('camera');
  var frame = document.getElementById('frame');

  camera.addEventListener('change', function (e) {
    var file = e.target.files[0];
    // Do something with the image file.
    frame.src = URL.createObjectURL(file);
  });
</script>
```

## format-detection とは

[レスポンシブデザインで気をつけたい、電話番号の扱い \- Qiita](https://qiita.com/emegane/items/bacdb2eaf9e1e7104720)

```html
<!-- 電話番号の自動リンク機能を無効化 -->
<meta name="format-detection" content="telephone=no" />

<!-- 電話番号にリンクを張る -->
<a href="tel:0312345678">電話はこちらへ</a>
```

## time 要素は、機械可読な形式で日付や時刻を提供するだけ

```html
<p>
  コンサートは
  <time datetime="2001-05-15T19:00">5月15日</time>
  に開催します。
</p>
```
