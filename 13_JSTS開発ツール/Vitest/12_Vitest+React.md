# Vitest+React

作成日 2025/10/22、更新日 2025/10/23

## 1. 解説記事を写経する

[React×TypeScriptではじめるVitest](https://zenn.dev/yskn_sid25/articles/b79d97a8f921d6)

### 新規プロジェクトを作成する

```bash
npm create vite@latest
# Project name: bacon
# Select a framework: React
# Select a variant: TypeScript
# Use rolldown-vite: No
# Install with npm and start now: No
cd bacon
npm install
npm run dev # Open browser to http://localhost:5173/
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

## 2. 公式サイトを読む

[@testing-library/jest-dom - npm](https://www.npmjs.com/package/@testing-library/jest-dom)

With Vitest

もしvitestを使っているならば、このモジュールはそのまま動作するはずであるが、テスト・セットアップ・ファイルで別のインポートを行う必要があるだろう。vitestの設定ファイルの`setupFi9les`プロパティにファイルが追加されるはずだ

```typescript
import '@testing-library/jest-dom/vitest'

setupFiles: ['./vitest-setup.js']
```

ローカルのセットアップによるが、tsconfig.jsonを更新する必要があるかもしれない

```json
{
    "compilerOptions": {
        "types": ["vitest/globals", "@testing-library/jest-dom"]
    },
    "include": [
        "./vitest-setup.js"
    ]
}
```
