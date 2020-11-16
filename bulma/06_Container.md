# Container を使う

作成日 2020/11/15

## 01. Container とは

主に `navbar`, `hero`, `section`, `footer` の直下に配置して、コンテンツのセンタリングを行う

[Container \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/layout/container/)

コンテナの横幅は`$device - (2 * $gap)`で、\$gap 変数のデフォルト値は 32px

デフォルトでは最大横幅が決まっているが、`is-fluid`をつけるとどこまでも大きくなる

## 02. サンプルコード

```html
<div class="container">
  <div class="notification is-primary">
    This container is <strong>centered</strong> on desktop and larger viewports.
  </div>
</div>
```
