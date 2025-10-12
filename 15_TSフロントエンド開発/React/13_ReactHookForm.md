# React Hook Form

作成日 2025/10/12

## 1. 解説記事を読む

[React Hook Form + Zod の紹介と導入【Next.js】](https://zenn.dev/b13o/articles/about-react-hook-form)

React Hook Formのメリット

1. コード量の削減
2. パフォーマンスの向上
3. 型安全性
4. クリーンな実装
5. エラーハンドリングの簡略化

ライブラリのインストール

```bash
npm i react-hook-form @hookform/resolvers zod
```

バリデーションスキーマの定義

```javascript
import z from "zod";

export const contactSchema = z.object({
    name: z.string().min(2, { message: "2文字以上" }),
    email: z.email({ messge: "有効なメルアド" }),
    message: z.string().max(1000, { message: "1000文字以内" }),
});

export type ContactFormValues = z.infer<typeof contactSchema>;
```

React Hook Formの実装

```javascript
import { useState } from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { contactSchema, ContactFormValues } from "./contactSchema";

function ContactForm() {
    const [ submitStatus, setSubmitStatus ] = useState<
        "idle" | "submitting" | "success" | "error"
    >("idle");

    const {
        register, handleSubmit,
        reset, formState: { errors }
    } = useForm<ContactFormValues>({
        resolver: zodResolver(contactShema),
        defaultValues: {
            name: "",
            email: "",
            message: "",
        }
    });

    const onSubmit = async (data: ContactFormValues) => {
        setSubmitStatus("submitting");
        try {
            // API呼び出し処理
            setSubmitStatus("success");
            reset();
        } catch (error) {
            setSubmitStatus("error");
        }
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
          <div>
            <label htmlFor="name">名前</label>
            <input id="name" {...register("name")} />
            {errors.name && <p>{errors.name.message}</p>}
          </div>
            {/* email, messageフィールドも同様 */}
          <button　type="submit" disabled={submitStatus === "submitting"}>
          {submitStatus === "submitting" ? "送信中..." : "送信する"}
          </button>
        </form>
    );
}
```

## 2. 公式サイト（英語）を読む

[React Hook Form - performant, flexible and extensible form library](https://react-hook-form.com/)

> React Hook Form reduces the amount of code you need to write while removing unnecessary re-renders.

[Get Started](https://react-hook-form.com/get-started)

ビデオが用意されている

[React Hook Form - Get Started](https://youtu.be/RkXv4AXXC_4)
