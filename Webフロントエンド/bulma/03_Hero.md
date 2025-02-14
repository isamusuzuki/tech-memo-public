# Hero を使う

作成日 2020/11/15

## 01. Hero とは

全幅を使って目立たせるバナー

[Hero \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/layout/hero/)

## 02. Hero の基本形

```html
<section class="hero">
  <div class="hero-body">
    <div class="container">
      <h1 class="title">Hero title</h1>
      <h2 class="subtitle">Hero subtitle</h2>
    </div>
  </div>
</section>
```

`is-fullheight`クラスをつけると 1 画面独占できる

hero の内部は、`hero-head`, `hero-body`, `hero-foot` の 3 パーツに分かれている

## 03. 過去に自分が採用したコード例

- hero を 2 つ用意して、`is-hidden-mobile`と`is-hidden-tablet`で切り分ける
- モバイルのときはメッセージを 3 行にして強制的に高さを出す
- モバイルは large も medium も高さが変わらない
- とにかく幅広の画像を用意する
- 用意した背景画像のサイズは、1024x415 で、これは 22:9 くらいの割合になっている

```html
<template>
  <div>
    <section class="hero is-medium has-bg-img is-hidden-mobile">
      <div class="hero-body">
        <div class="container">
          <h1 class="title white-color">
            あなたのホームページを<br />働くロボットにする
          </h1>
        </div>
      </div>
    </section>
    <section class="hero has-bg-img is-hidden-tablet">
      <div class="hero-body">
        <div class="container">
          <h1 class="title white-color is-size-4">
            あなたのHPを<br />働くロボット<br />にする
          </h1>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
  .has-bg-img {
    background-image: url('~static/hero.jpg');
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
  }
  .white-color {
    color: white;
  }
</style>
```
