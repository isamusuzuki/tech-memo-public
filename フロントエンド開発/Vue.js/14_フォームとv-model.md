# フォームでのv-modelの使い方

作成日 2025/04/28、更新日 2025/05/02

参照サイト => [【Vue.js】フォームでのv-modelの使い方と注意点](https://corecolors.net/v-model_form/)

## 1. テキスト(text)の場合

- `message`の型は、String

```html
<input type="text" v-model="message">
<p>{{message}}</p>
```

## 2. チェックボックス(checkbox)の場合

### 2a. チェック項目が「一つ」の場合

- `check`の型は、Boolean

```html
<label for="">
    <input type="checkbox" v-model="check">内容に同意する
</label>
```

### 2b. チェック項目が「複数」の場合

- `inputValue`の型はString[]、valueの値がpushされていく

```html
<label for="">
    <input type="checkbox" v-model="inputValue" value="メロン">メロン
</label>
<label for="">
    <input type="checkbox" v-model="inputValue" value="バナナ">バナナ
</label>
<label for="">
    <input type="checkbox" v-model="inputValue" value="みかん">みかん
</label>
```

## 3. ラジオボタン(radio)の場合

- `check`の型は、String。valueの値が入る
- 【注意】グループ内でラジオボタンは、1つしか同時に選択することができない

```html
<label for="">
    <input type="radio" v-model="check" value="内容に同意する">内容に同意する
</label>
<label for="">
    <input type="radio" v-model="check" value="内容に同意しない">内容に同意しない
</label>
```
