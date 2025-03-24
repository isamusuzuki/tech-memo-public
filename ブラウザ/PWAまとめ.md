# PWAまとめ

作成日 2018/02/18、最終更新日 2018/05/07

## manifest.jsonの正しい書き方とは

[ウェブアプリ マニフェスト  \|  Web  \|  Google Developers](https://developers.google.com/web/fundamentals/web-app-manifest/?hl=ja)

[Web App Manifest \| MDN](https://developer.mozilla.org/ja/docs/Web/Manifest)

```json
{
  "name": "ロボットゴン", /* 必ず含める要素 */
  "short_name": "Robot Gon", /* 必ず含める要素 */
  "description":"あなたのホームページを働くロボットにする。町田市・相模原市・八王子市のHP制作・SEO対策・Webプロモーションならロボットゴン",
  "lang":"ja",
  "background_color": "#ffffff",
  "theme_color": "#ffffff",
  "orientation": "portrait",

  /* 以下は@nuxt/pwaが自動生成する */
  "start_url": "/?standalone=true",
  "display": "standalone",
  "icons":[
    {"src":"/_nuxt/icons/icon_64.00000000000.png","sizes":"64x64","type":"image/png"},{"src":"/_nuxt/icons/icon_120.00000000000.png","sizes":"120x120","type":"image/png"},{"src":"/_nuxt/icons/icon_144.00000000000.png","sizes":"144x144","type":"image/png"},{"src":"/_nuxt/icons/icon_152.00000000000.png","sizes":"152x152","type":"image/png"},{"src":"/_nuxt/icons/icon_192.00000000000.png","sizes":"192x192","type":"image/png"},{"src":"/_nuxt/icons/icon_384.00000000000.png","sizes":"384x384","type":"image/png"},{"src":"/_nuxt/icons/icon_512.00000000000.png","sizes":"512x512","type":"image/png"}
  ]
}
```

マニュフェストを作成し、サイトに配置したら、すべてのページに次のlinkタブを設定する

```html
<link rel="manifest" href="/manifest.json">
```

Chrome DevTools > Application > Manifestタブでテストできる

## PWAとは

pwa = progressive web app

[PWA（Progressive Web Apps）の現状とその開発方法 \| フロントエンドBlog \| ミツエーリンクス](https://www.mitsue.co.jp/knowledge/blog/frontend/201702/23_2217.html)

> PWA（Progressive Web Apps）とは、ネイティブアプリ的な挙動をするWebサイトのことで、例えるならWebサイトとネイティブアプリを足して2で割ったような物と考えてもらえば解りやすいと思います。

PWAであるための条件

- HTTPSをサポートしていること
- manifest.jsonを含んでいること
- Service Workerを登録させること

これでホームスクリーンに登録できる。登録後にショートカットを起動すると、
ブラウザのアドレスバーや設定アイコンが取り除かれた状態でPWSが起動する

## Service Workerとは

[SafariとEdgeの開発版、Service Workerがデフォルトで利用可能に － Publickey](http://www.publickey1.jp/blog/17/safariedgeservice_worker.html)

> Web標準の1つである「Service Worker」は、Webブラウザで表示されるWebページのバックグラウンドで実行されるスクリプトです。Service Workerを利用することで、ネットワークに接続されていないオフラインのときでもプロキシサーバのようにWebページからのリクエストを処理することができるため、オフラインに対応したWebアプリケーションを実現することが可能になります。

[Service Worker の紹介  \|  Web  \|  Google Developers](https://developers.google.com/web/fundamentals/primers/service-workers/?hl=ja)

> Service Worker はブラウザが Web ページとは別にバックグラウンドで実行するスクリプトで、Web ページやユーザのインタラクションを必要としない機能を Web にもたらします。Service Worker は JavaScript Worker のひとつです。ですのでDOMに直接アクセスできません。Service Worker はプログラム可能なネットワークプロキシです。ページからのネットワークリクエストをコントロールできます。Service Worker は使用されていない間は終了され、必要な時になったら起動します。Service Worker は JavaScript の Promises を多用します。Service Worker は Web ページとはまったく異なるライフサイクルで動作します。

### 各ブラウザのService Workerサポート状況

[Can I use\.\.\. Support tables for HTML5, CSS3, etc](https://caniuse.com/#feat=serviceworkers)

- Edge: current 16 no, next 17 ok
- Firefox: current 58 ok
- Chrome: current 64 ok
- Safari: current 11 no, next 11.1 ok
- iOS Safari: current 11.2 no, next 11.3 ok
- Chrome for Android: current 64 ok

### Safariが次のバージョンからService Workerをサポート

[AppleもiOS/macOSをProgressive Web Apps（PWA）対応へ。次のSafari 11\.1でService Workerなど実装 － Publickey](http://www.publickey1.jp/blog/18/apple_ios_macos_progressive_web_apps.html)

> Appleが、iOSとmacOSの次バージョンにバンドルされるSafari 11.1で、Progressive Web Apps（PWA）の重要な構成要素であるService Workerをサポートすることが分かりました。次のiOSとmacOSのバージョンはiOS 11.3/macOS 10.13.4で、現在ベータ版としてAppleが開発中です。

[Workers at Your Service \| WebKit](https://webkit.org/blog/8090/workers-at-your-service/)

=> PWAをサポートするとは言ってないところがポイント

## PWA Builderとは

Microsoftは、次のバージョンのWindowsで、アプリストアにPWAを掲載する計画

[PWABuilder](https://www.pwabuilder.com/generator)

[PWABuilder Docs \| Documentation for PWABuilder\.com](http://docs.pwabuilder.com/)

[Welcoming Progressive Web Apps to Microsoft Edge and Windows 10 \- Microsoft Edge Dev BlogMicrosoft Edge Dev Blog](https://blogs.windows.com/msedgedev/2018/02/06/welcoming-progressive-web-apps-edge-windows-10/#vXKwlmEleWUYbeYR.97)
