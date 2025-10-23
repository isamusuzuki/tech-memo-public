# Vitestを試す

作成日 2025/10/22

## 1. Viteのプロジェクトを新規作成

```bash
cd ts-monorepo-dojo
npm create vite@latest
# Project name: daikon
# Select a framework: Vue
# Select a variant: TypeScript
# Use rolldown-vite: No
# Install with npm and start now: No
cd daikon
npm install
npm run dev # Open browser to http://localhost:5173/
```

## 2. Vitestをインストール

```bash
npm i -D vitest
```

## 3. 対象コードとテストコードを書く

src/helpers.ts

```typescript
export function increment(current: number, max: number = 10): number {
    if (current < max) {
        return current + 1;
    }
    return current;
}
```

src/helpers.test.ts

```typescript
import { describe, expect, test } from 'vitest';
import { increment } from './helpers';

describe('increment', () => {
    test('increments within the max limit', () => {
        expect(increment(5)).toBe(6);
        expect(increment(9)).toBe(10);
    });
    test('does not increment beyond the max limit', () => {
        expect(increment(10, 10)).toBe(10);
        expect(increment(15, 15)).toBe(15);
    });
    test('has a default max value of 10', () => {
        expect(increment(10)).toBe(10);
        expect(increment(11)).toBe(11);
    });
});
```

## 4. Vitestを実行する

```bash
npx vitest
#  Test Files  1 passed (1)
#       Tests  3 passed (3)
#    Start at  16:00:54
#    Duration  1.54s

npx vitest --coverage
# √ Do you want to install @vitest/coverage-v8? ... yes
# Package @vitest/coverage-v8@3.2.4 installed, re-run the command to start.

npx vitest --coverage
# % Coverage report from v8
#-----------------|---------|----------|---------|---------|-------------------
#File             | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
#-----------------|---------|----------|---------|---------|-------------------
#All files        |   14.28 |    83.33 |      75 |   14.28 |
# src             |      30 |       80 |   66.66 |      30 |
#  App.vue        |       0 |      100 |     100 |       0 | 2-14
#  helpers.ts     |     100 |      100 |     100 |     100 |
#  main.ts        |       0 |        0 |       0 |       0 | 1-5
# src/components  |       0 |      100 |     100 |       0 |
#  HelloWorld.vue |       0 |      100 |     100 |       0 | 2-34
#-----------------|---------|----------|---------|---------|-------------------
```

coverageフォルダにHTMLファイル群が生成されていた
