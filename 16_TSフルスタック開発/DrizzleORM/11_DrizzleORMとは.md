# Drizzle ORMとは

作成日 2025/09/08、更新日 2025/09/30

## 1. 公式サイト（英語）を読む

[Drizzle ORM - next gen TypeScript ORM.](https://orm.drizzle.team/)

## 2. 生のSQL文を使う

フルのSQLクエリー文を使うことができるという意味ではなくて、SQLの関数を紛れ込ませることができるということ

[Drizzle ORM - Magic sql`` operator](https://orm.drizzle.team/docs/sql)

> You can resort to using raw queries, which involve constructing a query as a raw string.
>
> With Drizzle’s sql template, you can go even further in crafting queries.

```javascript
// without sql<T> type defined
const response: { id: unknown }[] = await db.select({
    lowerName: sql`lower(${usersTable.id})`
}).from(usersTable);

// with sql<T> type defined
const response: { id: string }[] = await db.select({
    lowerName: sql<string>`lower(${usersTable.id})`
}).from(usersTable);
```
