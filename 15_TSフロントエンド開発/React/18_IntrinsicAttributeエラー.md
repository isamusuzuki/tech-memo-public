# IntrinsicAttributeエラーとは？

作成日 2025/10/25

## 1. 解説記事を読む

[IntrinsicAttributes をみたら冷静になってほしい #React - Qiita](https://qiita.com/guppy0356/items/3d069f2e03f726efa595)

> プロパティはオブジェクト（ = `props`）になるため、情報の取得は `props.プロパティ` が正しいです。`props` を毎回書く必要はないのでさらにプロパティを分割代入して、型の追加をします。

## 2. 実際にエラーが発生し、対処したコード

### 先に親コンポーネントを書き、後から子コンポーネントを書いた

src/App.tsx

```javascript
import SomeChild from './someChild';

function App() {
    const showCount = () => {};

    return (
        <>
            <SomeChild func={showCount} />
        </>
    );
}

export default App;
```

### IntrinsicAttributesエラーが発生

src/someChild.tsx

```javascript
const someChild = (func: () => void) => {
    return <div onClick={func}>SomeChild</div>;
};

export default someChild;
```

### 正しく書き直した

src/someChild.tsx

```javascript
const someChild = (props: { func: () => void }) => {
    return <div onClick={props.func}>SomeChild</div>;
};

export default someChild;
```
