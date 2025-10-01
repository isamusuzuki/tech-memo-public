# 例外のanyを避ける

作成日 2025/07/03

## 1. 問題

catchした例外に`any`をアサインするのは、なんとかして避けたい。どうしたらよいか？

## 2. 解決策

```javascript
try {
  // 略
} catch (err: unknown) {
  if (err instanceof Error) {
    console.error(`Error:`, err.message);
  } else {
    console.error(`Error:`, err);
  }
};
```

参考記事 => [TypeScript のエラーハンドリングを考える](https://qiita.com/frozenbonito/items/e708dfb3ab7c1fd3824d)
