# drizzle-kit pull

作成日 2025/09/08

## 1. 考えたこと

drizzleスキーマをTypeScriptで書けば、本番データベースへのマイグレーションを管理してくれ、drizzleスキーマでORMが機能するし、drizzleスキーマからは、Zodスキーマ／Open API仕様書を生成できるというのは素晴らしいアイデアだなと思ったが、すでに本番データベースがあり、別のマイグレーションシステムで管理している場合に、いちからdrizzleスキーマを書き直さないといけないの面倒だなと思った。逆向きのマイグレーションツールは用意されていないのだろうか？

## 2. 公式ドキュメント（英語）を読む

[drizzle-kit pull](https://orm.drizzle.team/docs/drizzle-kit-pull)

> `drizzle-kit pull` lets you literally pull(introspect) your existing database schema and generate `schema.ts` drizzle schema file, it is designed to cover database first approach of Drizzle migrations.

## 3. Getting Startedを読む

[Get Started with Drizzle and PostgreSQL in existing project](https://orm.drizzle.team/docs/get-started/postgresql-existing)

## 4. 本番サーバーのテーブルやスキーマは絞り込めるのか？

drizzle.config.ts

```javascript
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  dialect: "postgresql",
  schema: "./src/schema.ts",
  dbCredentials: {
    url: "postgresql://user:password@host:port/dbname",
  },
  extensionsFilters: ["postgis"],
  schemaFilter: ["public"],
  tablesFilter: ["*"],
});
```

- `tablesFilter` ... テーブル名のフィルター
- `schemaFilter` ... スキーマ名のフィルター
- `extensionsFilters` ... インストールされているデータベース拡張のリスト
