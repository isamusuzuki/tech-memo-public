# React Router

作成日 2025/10/13

## 1. 解説記事を読む

[【図解解説/初心者OK】Next.js不要？進化したReact Routerで技術記事アプリを作るチュートリアル【TypeScript/TailwindCSS】 #Remix - Qiita](https://qiita.com/Sicut_study/items/7dc1b0cdcc1bee210f05)

- React 19が安定版としてリリースされた。SSRなどサーバーコンポーネントがサポートされている
- React Router v7 => React RouterとRemixが統合された
- 今までどおり、ルーティングライブラリとしても利用できる
- フレームワークとしても使うことができる

## 2. 公式サイト（英語）を読む

[React Router Official Documentation](https://reactrouter.com/)

> A user‑obsessed, standards‑focused, multi‑strategy router you can deploy anywhere.

[React Router Home | React Router](https://reactrouter.com/home)

> There are three primary ways, or "modes", to use it in your app, so there are three guides to get you started.

[Picking a Mode | React Router](https://reactrouter.com/start/modes)

- Declarative ... 基本的なルーティング機能を提供する
- Data ... ローディング、アクション、待ちの状態などの機能を提供する
- Framework ... VitaプラグインでDataモードをラップして、全ての機能を提供する

### 2a. 日本語訳を発見

[React Router v7 ドキュメント 日本語訳](https://react-router-docs-ja.techtalk.jp/)

## 3. まずはDeclarativeモードを勉強する

### [Installation](https://reactrouter.com/start/declarative/installation)

インストール　=> `npm i react-router`

src/main.tsx

```javascript
import ReactDOM from "react-dom/client";
// import { StrictMode } from 'react';
import { BrowserRouter } from 'react-router';
import App from "./app";

const root = document.getElementById('root')!;

ReactDOM.createRoot(root).render(
    // <StrictMode>
    //     <App />
    // </StrictMode>
    <BrowserRouter>
        <App />
    </BrowserRouter>

);
```

### [Routing](https://reactrouter.com/start/declarative/routing)

src/main.tsx

```javascript
import ReactDOM from 'react-dom/client';
import { BrowserRouter, Route, Routes } from 'react-router';

import AboutPage from './pages/AboutPage';
import ArticlePage from './pages/ArticlePage';
import TopPage from './pages/TopPage';

const root = document.getElementById('root')!;

ReactDOM.createRoot(root).render(
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<TopPage />} />
            <Route path="/article" element={<ArticlePage />} />
            <Route path="/about" element={<AboutPage />} />
        </Routes>
    </BrowserRouter>
);
```

src/pages/TopPage.tsx（他も同様）

```javascript
export default function TopPage() {
    return <h1 className="m-3 p-2 text-3xl font-bold">Top Page</h1>;
}
```

### Nested Routes + Link

src/App2.tsx

```javascript
import { Link, Outlet } from 'react-router';

export default function App2() {
    return (
        <>
            <ul className="m-3 p-2">
                <li className="font-medium text-blue-600 hover:underline">
                    <Link to="/">トップ</Link>
                </li>
                <li className="font-medium text-blue-600 hover:underline">
                    <Link to="/article">公開記事</Link>
                </li>
                <li className="font-medium text-blue-600 hover:underline">
                    <Link to="/about">このサイトについて</Link>
                </li>
            </ul>
            <hr />
            <Outlet />
        </>
    );
}
```

src/main.tsx（一部）

```javascript
ReactDOM.createRoot(root).render(
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<App2 />}>
                <Route index element={<TopPage />} />
                <Route path="article" element={<ArticlePage />} />
                <Route path="about" element={<AboutPage />} />
            </Route>
        </Routes>
    </BrowserRouter>
);
```

### Link => NavLink

src/App2.tsx

```javascript
import { NavLink, Outlet } from 'react-router';

export default function App2() {
    return (
        <>
            <ul className="m-3 p-2">
                <li>
                    <NavLink
                        to="/"
                        className={({ isActive }) =>
                            isActive
                                ? 'text-blue-500 font-bold border-b-2 border-blue-500'
                                : 'text-gray-700 hover:text-blue-500'
                        }
                    >
                        トップ
                    </NavLink>
                </li>
                {/* 以下略 */}
            </ul>
            <hr />
            <Outlet />
        </>
    );
}
```
