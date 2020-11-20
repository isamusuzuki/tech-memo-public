# Sassまとめ

作成日 2018/03/08

## SassとScssの違い

```css
/* CSS */
.wrap.content{
  color: #000;
}

/*
  Sass
  入力文字を一番省略している
  インデントで依存関係を示す
*/
.wrap
　.content
    color: #000

/*
  Scss
  CSSに近い表記ができる
  括弧のネストで依存関係を示す
*/
.wrap{
　.content{
    color: #000;
　}
}
```

## Gulpを使ってScssをコンパイルする

[gulp-sass](https://www.npmjs.com/package/gulp-sass)を使う

```js
// gulpfile.js
const gulp = require('gulp')
const sass = require('gulp-sass')

gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass()).on('error', sass.logError)
    .pipe(gulp.dest('./css'))
})

gulp.task('sass:watch', function() {
  gulp.watch('./sass/**/*/scss', ['sass'])
})
```

## 『Sassの教科書（インプレス発行 2013/9/13）』を読む

```text
第3章 これだけはマスターしたいSassの基本機能

P.88 3-1 ルールのネスト
- ネストの基本
- 子孫セレクタ以外のセレクタを使うには
- @mediaのネスト

3-2 親セレクタの参照 &（アンパサンド）

3-3 プロパティのネスト

3-4 Sassで使えるコメント
// コンパイル後に残らないコメント
/* コンパイル後も残るコメント */

※ 3-5 変数と、3-6 演算は飛ばす

3-7 Sassの@import

P.112
- @importの概要
- CSSファイルを生成しない、パーシャル

Sassファイルのファイル名の最初に＿(アンダースコア）をつけると、
コンパイルしてもCSSファイルが生成されなくなる
```
