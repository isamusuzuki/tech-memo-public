# omit()メソッド

作成日 2025/10/11

## 1. 解説記事を読む

[Zodの omit で登録・編集フォームのバリデーションを分ける](https://qiita.com/masa_code/items/ef00353b94972a4c7309)

```javascript
export const userFormSchema = z.object({
  user_id: z
    .string()
    .nonempty('IDは必須です')
    .min(3, 'IDは3文字以上で入力してください')
    .regex(/^[a-zA-Z]+$/, 'IDは英単語のみ使用できます'),
  name: z
    .string()
    .nonempty('名前は必須です')
    .max(50, '名前は50文字以内で入力してください'),
  // ...他のフィールド
});

export type UserFormSchemaType = z.infer<typeof userFormSchema>;

// 編集用フォームのスキーマ（user_idを除外）
export const userEditFormSchema = userFormSchema.omit({ user_id: true });
export type UserEditFormSchemaType = z.infer<typeof userEditFormSchema>;
```

## 2. 公式ガイド（英語）を読む

[Defining schemas > `.pick()`](https://zod.dev/api?id=pick)

pick()とomit()が並んで紹介されている

TypeScriptの`Pick`と`Omit`というユーティリティタイプに触発されて、Zodもオブジェクトスキーマから特定のキーをピックしたりオミットしたりするAPIを提供する
