# htmxとは

作成日 2026/01/19

## 1. htmxを説明する

- aタグやformタグだけでなく、どんなタグでもHTTPリクエストを送信できるようにする
- クリックやフォーム送信だけでなく、どのようなイベントでもリクエストを送信できるようにする
- GETやPOSTだけでなく、どのようなHTTPメソッドも送信できるようにする
- サーバー側は、JSONではなく、HTMLで応答する

## 2. htmxの例

```html
<button hx-post="/clicked"
    hx-trigger="click"
    hx-target="#parent-div"
    hx-swap="outerHTML"
>
    クリックしてください！
</button>
```
