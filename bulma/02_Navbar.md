# Navbar を使う

作成日 2020/11/15

## 01. Navbar とは

ページの先頭にあるナビゲーションメニュー。レスポンシブになっていて、横幅が狭くなってくると、ブランドとハンバーガーアイコンだけになる

[Navbar \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/components/navbar/)

## 02. Navbar の構成

- navbar ... コンテナ
- navbar-brand ... 常に見えている。item と burger を含む
- navbar-burger ... brand の最後に置く。touch でのみ見える。is-active でバツになる
- navbar-menu ... dessktop では見えるが、touch では見えない。is-active でドロップダウンとして登場
- navbar-start ... menu の中に配置する。左に登場
- navbar-end ... menu の中に配置する。右に登場
- navbar-item ... アイテム単体、a タグもしくは div タグ
- navbar-link ... 矢印がつく
- navbar-dropdown ... ドロップダウンメニュー、item と divider を含む

```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <img
        src="https://bulma.io/images/bulma-logo.png"
        width="112"
        height="28"
      />
    </a>

    <a
      role="button"
      class="navbar-burger burger"
      aria-label="menu"
      aria-expanded="false"
      data-target="navbarBasicExample"
    >
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>
  <!-- /navbar-brand -->

  <div id="navbarBasicExample" class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item"> Home </a>

      <a class="navbar-item"> Documentation </a>

      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link"> More </a>

        <div class="navbar-dropdown">
          <a class="navbar-item"> About </a>
          <a class="navbar-item"> Jobs </a>
          <a class="navbar-item"> Contact </a>
          <hr class="navbar-divider" />
          <a class="navbar-item"> Report an issue </a>
        </div>
      </div>
    </div>
    <!-- /navbar-start -->

    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a class="button is-primary">
            <strong>Sign up</strong>
          </a>
          <a class="button is-light"> Log in </a>
        </div>
      </div>
    </div>
    <!-- /navbar-end -->
  </div>
  <!-- /navbar-menu -->
</nav>
```

## 03. navbar-burger と navbar-menu を連携させる

Bulma は CSS のみで構成されているため、「動き」は定義できない。JavaScript が必要

[Nuxt\.js に Bulma を組み込んだら、Vuex ストアが理解できた件 \- Qiita](https://qiita.com/isamusuzuki/items/5ec800e423a3a56ef03d)

## 04. Navbar をページトップに固定する

`is-fixed-top`クラスを追加するだけでなく、外側の body にも、`has-navbar-fixed-top` クラスを与える

`is-fixed-bottom`と`has-navbar-fixed-bottom`のコンビも使える

```html
<body class="has-navbar-fixed-top">
  <nav class="navbar is-fixed-top"></nav>
</body>
```

## 05. 常に navbar-menu を表示させる

- `navbar-menu`は、1024px 以下では隠れてしまう (is-hidden-touch)
- `navbar-burger`は、モバイルのみで登場する (is-hidden-desktop)
- `navbar-menu`に`is-active`をつけて常に登場させても、下に移動してしまう
- 常にナビゲーションメニューに表示されるのは、`navbar-brand`のみである

常にメニューを表示させる方法 => 以下の切り替えを行う

- デスクトップのときは navbar-menu で表示する
- モバイルのときは navbar-brand で表示する

```html
<nav class="navbar is-white">
  <div class="container">
    <div class="navbar-brand">
      <router-link to="/dashboard" class="navbar-item brand-text">
        電話会議招集システム
      </router-link>
      <div class="navbar-item is-hidden-desktop">
        <login-status></login-status>
      </div>
    </div>
    <div class="navbar-menu">
      <div class="navbar-end">
        <div class="navbar-item">
          <login-status></login-status>
        </div>
      </div>
    </div>
  </div>
</nav>
```
