# `sort()`の考え方

作成日 2025/04/30

```javascript
const numbers = [1,3,5,7,9];

// 昇順にする
numbers.sort((a, b) => {
  return a - b;
});

// 降順にする
stations.sort((a, b) => {
  return b - a;
});
```

- 書き方は、極めて単純。前に並べたいものを先にした引き算をリターンするだけ
- 動きは、値がプラスならばひっくり返す。値がマイナスならばそのまま
- `[1, 3]`の配列を例に考えてみる
- `a - b`のとき、`1 - 3 = -2`になるので、そのまま`[1,3]`となる
- `b - a`のとき、`3 - 1 = 2`になるので、ひっくり返して `[3,1]`となる
