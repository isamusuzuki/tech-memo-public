# React Routerとは

作成日 2025/09/16

## 1. 解説記事を読む

[【図解解説/初心者OK】Next.js不要？進化したReact Routerで技術記事アプリを作るチュートリアル【TypeScript/TailwindCSS】 #Remix - Qiita](https://qiita.com/Sicut_study/items/7dc1b0cdcc1bee210f05)

- React 19が安定版としてリリースされた。SSRなどサーバーコンポーネントがサポートされている
- React Router v7 = React RouterとRemixが統合された
- 今までどおり、ルーティングライブラリとしても利用できる
- フレームワークとしても使うことができる

## 2. 公式サイト（英語）を読む

[React Router Official Documentation](https://reactrouter.com/)

> A user‑obsessed, standards‑focused, multi‑strategy router you can deploy anywhere.

[React Router Home | React Router](https://reactrouter.com/home)

There are three primary ways, or "modes", to use it in your app, so there are three guides to get you started.

- Declarative
- Data
- Framework

[Picking a Mode | React Router](https://reactrouter.com/start/modes)

Declarative

```javascript
import { BrowserRouter } from "react-router";

ReactDOM.createRoot(root).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
);
```

Data

```javascript
import {
  createBrowserRouter,
  RouterProvider,
} from "react-router";

let router = createBrowserRouter([
  {
    path: "/",
    Component: Root,
    loader: loadRootData,
  },
]);

ReactDOM.createRoot(root).render(
  <RouterProvider router={router} />,
);
```

Framework

routes.ts

```javascript
import { index, route } from "@react-router/dev/routes";

export default [
  index("./home.tsx"),
  route("products/:pid", "./product.tsx"),
];
```

product.tsx

```javascript
import { Route } from "./+types/product.tsx";

export async function loader({ params }: Route.LoaderArgs) {
  let product = await getProduct(params.pid);
  return { product };
}

export default function Product({
  loaderData,
}: Route.ComponentProps) {
  return <div>{loaderData.product.name}</div>;
}
```
