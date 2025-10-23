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
# Install with npm and start now: No
cd banana
npm install
npm run dev # Open browser to http://localhost:5173/
```

## 1. useState

- count変数を用意しただけでは、その値が変わっても、画面の再描画は行われない
- setCount関数を使うことで、新しい値をセットした時に、画面の再描画が行われるようになる

banana/src/App.tsx

```javascript
import { useState } from 'react';
import './App.css';

function App() {
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setCount(count + 1);
    };

    return (
        <>
            <h1>useState</h1>
            <button onClick={handleClick}>+</button>
            <p>{count}</p>
        </>
    );
}

export default App;
```

## 2. useEffect

- useEffectの第2引数が空配列の場合、マウント時に（第1引数の）コールバック関数が実行される
- StrictModeで、かつデバッグの場合、コールバック関数は2回実行される
- 第2引数に`count`を入れると、count変数が変更になる度に、コールバック関数が実行されるようになる
- 第2引数に`count`を入れて、コールバック関数の中でsetCountを使うと無限ループが発生する。要注意

banana/src/App.tsx

```javascript
import { useEffect } from 'react';

function App() {

    useEffect(() => {
        console.log('Hello, Hooks');
    }, []);

    return (
        <>
            <h1>useEffect</h1>
        </>
    );
}

export default App;
```

## 3. useContext

- propsを使わずに、親から子へ孫へ、データを渡すことができる
- Reduxよりも使い方が簡単

banana/src/main.tsx

```javascript
import { createContext, StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import App from './App.tsx';
import './index.css';

const personalInfo = {
    name: 'Ichiro',
    age: 51,
};

const PersonalInfoContext = createContext(personalInfo);

createRoot(document.getElementById('root')!).render(
    <PersonalInfoContext.Provider value={personalInfo}>
        <StrictMode>
            <App />
        </StrictMode>
    </PersonalInfoContext.Provider>
);

export default PersonalInfoContext;
```

banana/src/App.tsx

```javascript
import { useContext } from 'react';
import './App.css';
import PersonalInfoContext from './main';

function App() {
    const personalInfo = useContext(PersonalInfoContext);

    return (
        <>
            <h1>useContext</h1>
            <p>{personalInfo.name}</p>
            <p>{personalInfo.age}</p>
        </>
    );
}

export default App;
```

## 4.useRef

- useRefは参照を保持するためのフック。Refオブジェクトは`{ current: initialValue }`を生成し、参照可能とする
- 圧倒的に多い使い方が、HTML要素のref属性にuseRefの値を渡して、HTML要素を参照すること

banana/src/App.tsx

```javascript
import { useRef } from 'react';
import './App.css';

function App() {
    const inputElement = useRef<HTMLInputElement>(null);

    const handleRef = () => {
        console.log(inputElement.current?.value);
        console.log(inputElement.current?.offsetHeight);
        inputElement.current?.focus();
    };

    return (
        <>
            <h1>useRef</h1>
            <input type="text" ref={inputElement} />
            <button onClick={handleRef}>UseRef</button>
        </>
    );
}

export default App;
```

## 5. useReducer

- useStateよりも、複雑な状態プロパティをまとめて扱う場合に適している
- Reduxのシンプルな代替
- [React Hook FormをやめてuseReducerを使用した話](https://zenn.dev/makumattun/articles/a1a4477a1a5e6c)

```javascript
import { useReducer } from 'react';
import './App.css';

const reducer = (state: number, action: { type: string }): number => {
    switch (action.type) {
        case 'increment':
            return state + 1;
        case 'decrement':
            return state - 1;
        default:
            return state;
    }
};

function App() {
    const [state, dispatch] = useReducer(reducer, 0);

    return (
        <>
            <h1>useReducer</h1>
            <p>カウント: {state}</p>
            <button onClick={() => dispatch({ type: 'increment' })}>+</button>
            <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
        </>
    );
}

export default App;
```

## 6. useMemo

32:08以降
