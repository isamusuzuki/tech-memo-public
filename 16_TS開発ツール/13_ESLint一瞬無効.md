# ESLintを一瞬無効にする

作成日 2025/09/22

公式サイト（英語） => [Disabling Rules](https://eslint.org/docs/latest/use/configure/rules#disabling-rules)

## そのファイルだけ、特定のルールを無効にする

- ファイルの先頭に以下のコメントを書く
- `eslint-disable` だけを書くとすべてのルールが無効になるので、これはNG
- 必ず無効にしたいルールを書き込むことを推奨する

```javascript
/* eslint-disable */

/* eslint-disable @typescript-eslint/no-explicit-any */

// 以下の書き方も同じ効果あり

/* eslint @typescript-eslint/no-explicit-any: off */
/* eslint @typescript-eslint/no-explicit-any: 0 */
```

## 特定の行だけ、ルールを無効にする

- 探しにくくなるので、非推奨

```javascript
// eslint-disable-line

// eslint-disable-line no-unused-vars
```

## 次の1行だけ、ルールを無効にする

- 探しにくくなるので、非推奨

```javascript
// eslint-disable-next-line

// eslint-disable-next-line no-undef
```
