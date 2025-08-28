# LogTape導入

作成日 2025/08/28

## 1. LogTapeとは

JavaScriptとTypeScript向けの新しいロギングライブり。Bunをサポートする

公式サイト => [LogTape](https://logtape.org/)

インストール => `bun add @logtape/logtape`

## 2. サンプルコード

test.ts

```javascript
import { configure, getConsoleSink, getLogger, getTextFormatter } from '@logtape/logtape';

import { program } from 'commander';

await configure({
    sinks: {
        console: getConsoleSink({ formatter: getTextFormatter({timestamp: "time-tz", level: "full"}) })
    },
    loggers: [
        { category: 'test', lowestLevel: 'info', sinks: ['console'] },
        { category: ['logtape', 'meta'], lowestLevel: 'error', sinks: ['console'] }
    ]
});

program
    .name('bun run test.ts')
    .description('これはテスト')
    .version('0.0.1')
    .action(async () => {
        const logger = getLogger('test');
        logger.info('Hello, Test!');
    });

program.parse(process.argv);
```
