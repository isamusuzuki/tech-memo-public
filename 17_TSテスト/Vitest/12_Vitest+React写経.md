# Vitest+React

作成日 2025/10/22、更新日 2025/11/30

## 解説記事を写経する

[【初心者完全版】0からReactを始めてCI/CD構築までできるチュートリアル【TypeScript/GitHubActions/Vitest/Firebase】 #React - Qiita](https://qiita.com/Sicut_study/items/9792f2f01dc887d4cb31)

## 新規プロジェクトを作成する

```bash
npm create vite@latest
# Project name: daikon
# Select a framework: React
# Select a variant: TypeScript
# Use rolldown-vite: No
# Install with npm and start now: No
cd daikon
npm install
npm run dev # Open browser to http://localhost:5173/
```

## Vitest他のインストール

```bash
npm i -D vitest
npm i -D jsdom
npm i -D @testing-library/dom @testing-library/jest-dom @testing-library/react
```

## Vitestの設定ファイル

vitest-setup.ts

```javascript
import '@testing-library/jest-dom';
```

vitest.config.ts

```javascript
import react from '@vitejs/plugin-react';
import { defineConfig } from 'vitest/config';

export default defineConfig({
    plugins: [react()],
    test: {
        environment: 'jsdom',
        globals: true,
        setupFiles: ['./vitest-setup.ts'],
    },
});
```

## 追加設定

tsconfig.app.jsonに以下の1行を追加

```json
{
    "compilerOptions": {
        "types": [
            "@testing-library/jest-dom"
        ]
    }
}
```

## テストコードの作成

`src/__tests__/App.test.tsx`

```javascript
import '@testing-library/jest-dom';
import { render, screen } from '@testing-library/react';
import { describe, expect, test } from 'vitest';
import App from '../App';

describe('App', () => {
    test('アプリタイトルが表示されている', () => {
        render(<App />);
        expect(screen.getByRole('heading', { name: 'Vite + React' })).toBeInTheDocument();
    });
});
```

## テスト実行

```bash
npx vitest
#  ✓ src/__tests__/App.test.tsx (1 test) 62ms
#  Test Files  1 passed (1)
#       Tests  1 passed (1)
```

## Todoアプリのテストコード完成版

```javascript
import '@testing-library/jest-dom';
import { fireEvent, render, screen, within } from '@testing-library/react';
import { describe, expect, test } from 'vitest';
import App from '../App';

describe('App Component', () => {
    test('アプリタイトルが表示されている', () => {
        render(<App />);
        expect(screen.getByRole('heading', { name: '📝 Todoアプリ!' })).toBeInTheDocument();
    });

    test('TODOを追加することができる', () => {
        render(<App />);

        const input = screen.getByRole('textbox', { name: '新しいタスクを入力' });
        const addButton = screen.getByRole('button', { name: '追加' });

        fireEvent.change(input, { target: { value: 'テストタスク' } });
        fireEvent.click(addButton);

        const list = screen.getByRole('list');
        expect(within(list).getByText('テストタスク')).toBeInTheDocument();
    });

    test('TODOを完了にすることができる', () => {
        render(<App />);

        const input = screen.getByRole('textbox', { name: '新しいタスクを入力' });
        const addButton = screen.getByRole('button', { name: '追加' });

        fireEvent.change(input, { target: { value: '完了テストタスク' } });
        fireEvent.click(addButton);

        const checkboxes = screen.getAllByRole('checkbox');
        const lastCheckbox = checkboxes[checkboxes.length - 1];
        fireEvent.click(lastCheckbox);

        expect(lastCheckbox).toBeChecked();
    });

    test('完了したTODOの数が表示されている', () => {
        render(<App />);

        const input = screen.getByRole('textbox', { name: '新しいタスクを入力' });
        const addButton = screen.getByRole('button', { name: '追加' });

        fireEvent.change(input, { target: { value: 'タスク1' } });
        fireEvent.click(addButton);

        fireEvent.change(input, { target: { value: 'タスク2' } });
        fireEvent.click(addButton);

        const checkboxes = screen.getAllByRole('checkbox');
        fireEvent.click(checkboxes[0]);

        expect(screen.getByText('完了済み: 1 / 2')).toBeInTheDocument();
    });

    test('空のTODOは追加されない', () => {
        render(<App />);

        const input = screen.getByRole('textbox', { name: '新しいタスクを入力' });
        const addButton = screen.getByRole('button', { name: '追加' });

        fireEvent.change(input, { target: { value: '' } });
        fireEvent.click(addButton);

        expect(screen.getByText('タスクがありません')).toBeInTheDocument();
    });
});
```
