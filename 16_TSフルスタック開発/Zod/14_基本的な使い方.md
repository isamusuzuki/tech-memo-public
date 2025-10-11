# Zodの基本的な使い方

作成日 2025/10/08

## 公式ドキュメント（英語）を読む

[Basic usage | Zod](https://zod.dev/basics)

```javascript
import * as z from "zod";

// スキーマを定義する
const Player = z.object({
    username: z.string(),
    xp: z.number()
});

// データをパースする
Player.parse({ username: "billie", xp: 100 });
// => returns { username: "billie", xp: 100 }

// エラーを対処する1
try {
    Player.parse({ username: 42, xp: "100" });
} catch(error){
    if(error instanceof z.ZodError){
        error.issues;
    /* [
      {
        expected: 'string',
        code: 'invalid_type',
        path: [ 'username' ],
        message: 'Invalid input: expected string'
      },
      {
        expected: 'number',
        code: 'invalid_type',
        path: [ 'xp' ],
        message: 'Invalid input: expected number'
      }
    ] */
    }
}

// エラーを対処する2
const result = Player.safeParse({ username: 42, xp: "100" });
if (!result.success) {
  result.error;   // ZodError instance
} else {
  result.data;    // { username: string; xp: number }
}

// 型に利用する
type Player = z.infer<typeof Player>;
const player: Player = { username: "billie", xp: 100 };
```
