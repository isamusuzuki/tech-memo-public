# Form を使う

作成日 2020/11/16

## 01. Form とは

ユーザーが操作したり、入力したりするフォームをデザインする

[Form \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/form/)

- フォーム要素は、`control`コンテナでラップする
- `field`クラスをコントロールのコンテナとして使うと適度なスキマができる

```html
<div class="field">
  <label class="label">Name</label>
  <div class="control">
    <input class="input" type="text" placefolder="Text Input" />
  </div>
</div>
```

### Checkbox をデザインする

[Checkbox \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/form/checkbox/)

```html
<label class="checkbox">
  <input type="checkbox" />
  Remember me
</label>
```
