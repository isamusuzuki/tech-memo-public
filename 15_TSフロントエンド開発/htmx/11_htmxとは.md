# htmxとは

作成日 2025/10/09、更新日 2025/10/15

## 1. 解説記事を読む

[もうjsなんていらない！世界で流行っているHTMXについてまとめてみた](https://qiita.com/twrcd1227/items/7bce18167fb02ec22729)

> HTMXは、JavaScript を記述せずに、Ajax通信や高度なUXを実現できるライブラリ。

サンプルコード

```html
<script src="https://unpkg.com/htmx.org@1.9.10"></script>
<h2>シンプルな例</h2>
<p id="target">クリックしたらここが変わるよ</p>
<button hx-get="https://jsonplaceholder.typicode.com/posts/1" hx-target="#target" hx-trigger="click">クリック</button>
```

## 2. SpringBoot+ThymeleafにHTMXを組み合わせた解説記事をいくつか読む

[htmxをSpringBoot Thymeleafで試してみる(1) - エキサイト TechBlog.](https://tech.excite.co.jp/entry/2023/02/02/181451)

もっとも簡単な例。Postリクエストをサーバー側で受け取り、HTMLを返す

[SpringBootとhtmx（その1：導入編） #HTMX - Qiita](https://qiita.com/alpha_pz/items/90576ee920ff91e59945)

> 商品データの一覧画面を作り、ページングや詳細画面を出す画面も用意します。画面レイアウトやUIはBootstrap5.3を使います。

[レガシーWebアプリケーションをhtmxへ切り替えた話 - asoview! Tech Blog](https://tech.asoview.co.jp/entry/2024/10/18/095224)

> 「実行中の非同期処理」と、「非同期処理の結果」で分割し、さらにこの画面全体を呼び出す実装を行うControllerに変更します。
>
> モーダルダイアログの表示や内容の切り替えも、htmxで簡単に置き換えができ、さらに実装も簡素にできます。

## 3. 公式サイト（英語）を読む

[</> htmx - high power tools for html](https://htmx.org/)

> htmx gives you access to AJAX, CSS Transitions, WebSockets and Server Sent Events directly in HTML, using attributes, so you can build modern user interfaces with the simplicity and power of hypertext
>
> htmx is small (~16k min.gz’d), dependency-free, extendable & has reduced code base sizes by 67% when compared with react

[</> htmx ~ Examples](https://htmx.org/examples/)

## 4. Hotwire vs. htmx

[</> htmx ~ Hotwire / Turbo ➡️ htmx Migration Guide](https://htmx.org/migration-guide-hotwire-turbo/)

このガイドの目的は、htmxが持つHotwireと同等の機能を紹介することである

## 5. Hono + htmxのサンプルコードを発見する

[Hono + htmx + Cloudflareでアプリケーションを実装してみる](https://zenn.dev/aoito/articles/dc111b83212c0b)

[Hono + htmx + Cloudflareは新しいスタック](https://zenn.dev/yusukebe/articles/e8ff26c8507799)

[yusukebe/hono-htmx: Hono+htmx stack](https://github.com/yusukebe/hono-htmx)
