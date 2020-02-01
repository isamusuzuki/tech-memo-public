# Nest.js

作成日 2019/12/03

## 01. Nest.js とは

TypeScript で実装された、サーバーサイドの Node.js フレームワーク。Angular に近い構造をしている

公式サイト => [NestJS \- A progressive Node\.js web framework](https://nestjs.com/)

`main.ts`ファイル

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
    const app = await NestFactory.create(AppModule);
    app.setViewEngine('hbs');
    await app.listen(3000);
}
bootstrap();
```

ドキュメント => [Documentation \| NestJS \- A progressive Node\.js web framework](https://docs.nestjs.com/)

プロジェクトの開始

```bash
git clone https://github.com/nestjs/typescript-starter.git project
cd project
npm install
npm run start
# => ブラウザで、http://localhost:3000/を開く
```

## 02. Auth0 との統合について解説記事を写経する

[Full\-Stack TypeScript Apps: Developing a Secure API with NestJS](https://auth0.com/blog/authors/dan-arias/)
