# IMAGE 関数

作成日 2022/08/31

インターネットの画像をセルに挿入する

解説記事を発見 => [「ExcelAPI」と新関数「IMAGE」を組み合わせたデモが早速お披露目 - やじうまの杜 - 窓の杜](https://forest.watch.impress.co.jp/docs/serial/yajiuma/1435866.html)

解説記事 2 => [「Excel」に新関数「IMAGE」が追加 ～セルへインターネットの画像を簡単に挿入できる - 窓の杜](https://forest.watch.impress.co.jp/docs/news/1434913.html)

## IMAGE 関数の構文

```text
= IMAGE(source, [alt_text], [sizing], [height], [width])
```

- sizing ... 画像のサイズを指定
  - 0 ... 縦横比を維持してセルの大きさに合わせる
  - 1 ... 縦横比を無視してセルいっぱいに表示
  - 2 ... 元のイメージサイズを維持
  - 3 ... height,width引数に従う
