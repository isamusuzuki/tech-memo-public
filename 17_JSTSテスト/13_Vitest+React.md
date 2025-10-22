# Vitest + React

作成日 2025/10/22

## 解説記事を写経する

[React×TypeScriptではじめるVitest](React×TypeScriptではじめるVitest)

### 新規プロジェクトを作成する

```bash
npm create vite@latest
# Project name: bacon
# Select a framework: React
# Select a variant: TypeScript
# Use rolldown-vite: No
# Install with npm and start: Yes
# => http://localhost:5173/

cd bacon
code .
```

### Vitestのインストール

```bash
npm i -D vitest
npm i -D jsdom @testing-library/react @testing-library/jest-dom
```

### Vitestの設定ファイル

vitest.config.ts

```typescript
import { defineConfig } from 'vitest/config';

export default defineConfig({
    test: {
        include: ['tests/**/*.test.tsx'],
        environment: 'jsdom',
    }
});
```

### テストコードの作成

tests/App.test.tsx

```typescript
import '@testing-library/jest-dom';
import { render, screen } from '@testing-library/react';
import { expect, test } from 'vitest';
import App from '../src/App';

test('renders h1 text', () => {
    render(<App />);
    const headerElement = screen.getByText("Vite + React");
    expect(headerElement).toBeInTheDocument();
});
```

### テスト実行

```bash
npx vitest
# FAIL  tests/App.test.tsx [ tests/App.test.tsx ]
# ReferenceError: expect is not defined
```

なぜか失敗する。node_modules/@testing-library/jest-dom/dist/index.mjsにあるexpectが定義されていない

jestをインストールしても解決しない
