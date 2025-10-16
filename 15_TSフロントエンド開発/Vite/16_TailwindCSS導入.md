# Tailwind CSSを導入する

作成日 2025/04/28

公式ガイド（英語） => [Get started with Tailwind CSS](https://tailwindcss.com/docs/installation/using-vite)

Tailwind CSSをインストールする

```bash
npm i -D tailwindcss @tailwindcss/vite
```

vite.config.tsファイルを編集する

```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    react(),
    tailwindcss()
  ]
})
```

Tailwind CSSを組み込む

```text
--app/
    `--src/
        |--App.tsx
        |--index.css
        `--main.tsx
```

src/index.css

```css
@import "tailwindcss";
```

src/main.tsx

```javascript
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.tsx'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

src/App.tsx

```javascript
function App() {
    return (
        <h1 className="m-3 p-2 text-3xl font-bold underline">React道場</h1>
    )
}

export default App
```
