# フォームでのv-modelの使い方

作成日 2025/04/28

参照サイト => [【Vue.js】フォームでのv-modelの使い方と注意点](https://corecolors.net/v-model_form/)

## 1. テキスト(text)の場合

```html
<input type="text" v-model="message">
<p>{{message}}</p>
<!--messageの型はString-->
```

## 2. チェックボックス(checkbox)の場合

### 2a. チェック項目が「一つ」の場合

```html
<label for="">
    <input type="checkbox" v-model="check">内容に同意する
</label>
<!--checkの型はBoolean-->
```

### 2b. チェック項目が「複数」の場合

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
<!--inputValueの型はString[]、valueの値がpushされていく-->
```

## 3. ラジオボタン(radio)の場合

```html
<label for="">
    <input type="radio" v-model="check" value="内容に同意する">内容に同意する
</label>
<!--checkの型はString、valueの値が入る-->
```
