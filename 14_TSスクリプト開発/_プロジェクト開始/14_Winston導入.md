# Winston導入

作成日 2025/12/18

## 1. Winstonとは

最も人気のあるNode.jsのロギングライブラリ

npmサイト => [winston - npm](https://www.npmjs.com/package/winston)

インストール => `bun add winston`

## 2. サンプルコード

index.ts

```javascript
import { clean3 } from '@/utils/clean3.ts';
import winston from 'winston';

const timezoned = () => {
    return new Date().toLocaleString('ja-JP', {
        timeZone: 'Asia/Tokyo',
    });
};

const logger = winston.createLogger({
    level: 'info',
    format: winston.format.combine(
        winston.format.timestamp({
            format: timezoned,
        }),
        winston.format.printf((info) => `${info.timestamp} [${info.level}] ${info.message}`)
    ),
    transports: [new winston.transports.Console(), new winston.transports.File({ filename: 'logs/index.log' })],
});

program.name('bun start').description('作業').version('0.0.1');

program.command('clean3').action(async () => {
    await clean3(logger);
});

program.parse(process.argv);
```

src/utils/clean3.ts

```javascript
import type { Logger } from 'winston';

export async function clean3(logger: Logger): Promise<void> {
    logger.info('Clean 3');
}
```

## 3. LogTapeとWinstonの違い

LogTapeのタイムスタンプは、常にUTCで、日本時間に変更する方法がわからなかった

```text
2025-12-18 04:59:15.911 +00:00 [INF] index: Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\instructions
2025-12-18 04:59:15.915 +00:00 [INF] index: Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\markdown
2025-12-18 04:59:15.917 +00:00 [INF] index: Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\temp
```

Winstonのタイムスタンプは、タイムゾーンを日本時間に変更することができたが、フォーマットが気にいった形にならなかった

```text
2025/12/18 13:48:05 [info] Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\instructions
2025/12/18 13:48:05 [info] Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\markdown
2025/12/18 13:48:05 [info] Cleaned folder: C:\Users\isuzuki\workspaces\nodejs-ast-sagyo\temp
```
