# null 判定

作成日 2021/01/27

## 01. びっくりマークを使う

以下の条件式が真になるのは、`null`, `undefined`, `0`, `空文字`, `false` のとき

これだと数値のゼロはオーケーなときに困る

```javascript
if (!hoge) {
  // hogeがない場合
}
```

## 02. イコール記号3つを使う

以下の条件式が真になるのは、`null`, `undefined` のときだけ

存在チェックはこちらのほうが安全

```javascript
if (hoge === null || hoge === undefined) {
  // hogeがない場合
}
```
