# 「React Hooks 入門」を写経する

作成日 2025/10/22

[【React Hooks入門】完全初心者OK！8種類のHooksを学んでReactの理解を深めよう - YouTube](https://www.youtube.com/watch?v=uuAdVs7sbAs)

## 0. 新しいプロジェクトを作成する

```bash
npm create vite@latest
# Project name: banana
# Select a framework: React
# Select a variant: TypeScript
# Use rolldown-vite: No
# Install with npm and start: Yes

cd banana
code .
```

## 1. UseState

banana/src/App.tsx

```typescript
import { useState } from 'react';
import './App.css';

function App() {
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setCount(count + 1);
    };

    return (
        <>
            <h1>UseState</h1>
            <button onClick={handleClick}>+</button>
            <p>{count}</p>
        </>
    );
}

export default App;
```

- count変数があるだけでは、その値が変わっても、画面の再描画は行われない
- setCount関数を使うことで、新しい値をセットした時に、画面の再描画が行われるようになる

## 2. UseEffect

```typescript
import { useEffect } from 'react';

function App() {

    useEffect(() => {
        console.log('Hello, Hooks');
    }, []);

    return (
        <>
            <h1>UseEffect</h1>
        </>
    );
}

export default App;
```

- useEffectの第2引数が空配列の場合、マウントされた時にコールバック関数が実行される
- StrictModeかつデバッグの時、useEffectのコールバック関数は2回実行される
- 第2引数に`count`を入れると、count変数が変更になる度に、コールバック関数が実行されるようになる
- 第2引数に`count`が入った状態で、コールバック関数でsetCountを使うと無限ループが発生する。要注意

ここまで14:20
