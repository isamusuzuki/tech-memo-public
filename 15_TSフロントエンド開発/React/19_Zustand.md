# Zustand

作成日 2025/10/27

## 1. 解説記事を読む

[状態管理ライブラリ Zustand の紹介と導入【React】](https://zenn.dev/b13o/articles/tutorial-zustand)

### Zustandとは？

- Zustand は、React アプリケーションのための、軽量な状態管理ライブラリ
- useStateだけでは（コンポーネントツリーが複雑になった際の対処が）不十分
- useContextではパフォーマンスが悪化し、Providerタワーが発生する
- グローバルな状態管理には、useContextでの実装を頑張るより、ライブラリを導入する

### Zustandの導入手順

インストール => `npm i zustand`

store/useStore.jsx

```javascript
import { create } from "zustand";

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

export default useStore;
```

コンポーネントからフックを使う

```javascript
const App = () => {
  const { count, increment } = useStore();
  return (
    <div>
       <p>{count}</p>
       <button onClick={increment}>+1</buttoon>
    </div>
  );
};
```

特定の値や操作を指定して読み込むことも可能

```javascript
const IncrementButton = () => {
  const increment = useStore((state) => state.increment);
  return (
    <div>
       <button onClick={increment}>+1</buttoon>
    </div>
  );
};
```

## 2. 公式ドキュメント（英語）を読む

[Introduction - Zustand](https://zustand.docs.pmnd.rs/getting-started/introduction)
