# [Parcel](https://parceljs.org/)とは

作成日 2019/03/08

## 日本語の記事

[webpack に疲れた僕を救ってくれた parcel がすばらしい \- Qiita](https://qiita.com/Nekonecode/items/ab342eaaf8985190997d)

[Parcel を利用して簡単 Vue\-Element\-UI 実装 \- Qiita](https://qiita.com/mastar_3104/items/67a0e9e227d57b99ab8a)

## [公式の Getting Started](https://parceljs.org/getting_started.html)を読む

```bash
npm init -y
npm install --save-dev

# index.htmlとindex.jsファイルを作成した後
parcel index.html
#=> http://localhost:1234/ を開いて動作確認
#=> -p <ポート番号>オプションで、ポート番号の変更が可能

parcel watch index.html
#=> Webサーバーは起動しないが、
#=> ファイルを変更したら自動的に再ビルドする

# 本番用にビルドする
parcel build index.html
```

## Vue をコンパイルしたら警告が出た

[Vue 2\.0 warn:You are using the runtime\-only build of Vue where the template compiler is not available \- Get Help \- Vue Forum](https://forum.vuejs.org/t/vue-2-0-warn-you-are-using-the-runtime-only-build-of-vue-where-the-template-compiler-is-not-available/9429)

ES Module を使っていてコンパイルしようとしている時は、`vue.js`ではなく、`vue.esm.js`を使う必要がある

[vue/dist at dev · vuejs/vue · GitHub](https://github.com/vuejs/vue/tree/dev/dist)

### 解決策

package.json にアリアスを追加したら解決できた

```json
{
  "alias": {
    "vue": "./node_modules/vue/dist/vue.esm.js"
  }
}
```

## [CSS](https://parceljs.org/css.html)もコンパイルできる

エントリーファイルに書き込むだけ

```html
<link rel="stylesheet" type="text/css" href="index.css" />
```

```css
@import "../node_modules/bulma/css/bulma.min.css";
```

## XAMPP の API にアクセスするには

### CORS エラーを回避する必要あり

`httpd.conf`の`<Directory></Directory>`内に、
`Header set Access-Control-Allow-Origin "*"`という 1 行を追加する

### プリフライトリクエストのエラーにも対処する必要あり

実は、POST メソッドのリクエストの前に、OPTIONS メソッドが走っている

`Header set Access-Control-Allow-Headers "Content-Type"`を追加する

### Authorization ヘッダーを使うには、さらに設定が必要

`Header set Access-Control-Allow-Credentials "true"`を追加する

Authorization ヘッダーの使用に対する許可も必要

### 最終形

```text
    Header set Access-Control-Allow-Origin "http://localhost:1234"
    Header set Access-Control-Allow-Methods "GET, POST, DELETE, OPTIONS"
    Header set Access-Control-Allow-Headers "Content-Type, Authorization"
    Header set Access-Control-Allow-Credentials "true"
```

## public フォルダにもっていったときに、URL を修正しなくて済むようにするには

build にだけオプションを追加する

```json
{
  "scripts": {
    "dev:kaigi": "parcel kaigi/index.html",
    "build:kaigi": "parcel build kaigi/index.html --public-url /html/kaigi"
  }
}
```

dev と build のコマンドを毎回、書き換えるのは面倒臭いので、dev:kaigi, build:kaigi として、
以降、コマンド名を増やせるようにする

## ファイルを吐き出す先である dist フォルダがどんどん汚れてくる

毎回 rimraf でキレイにするのよい

`npm install rimraf --save-dev`

```json
{
  "scripts": {
    "clean": "rimraf dist"
  },
  "devDependencies": {
    "rimraf": "^2.6.3"
  }
}
```

## Font Awesome は CDN を使い続ける

`npm install --save @fortawesome/fontawesome-free`で、ダウンロードできるがビルドすると、Web フォントも吐き出すことになる。ファイルが増えて面倒臭いので、CDN を使い続けることにした
