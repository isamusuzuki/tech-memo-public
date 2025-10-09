# HTMXとは

作成日 2025/10/09

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
