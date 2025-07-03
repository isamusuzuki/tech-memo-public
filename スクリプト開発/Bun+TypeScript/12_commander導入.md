# commander導入

作成日 2025/05/01、更新日 2025/07/03

## 1. commanderとは

Node.js で、CLI プログラムを完成させるための便利ツール

公式 => [tj/commander.js: node.js command-line interfaces made easy](https://github.com/tj/commander.js)

インストール => `bun add commander`

## 2. commanderサンプルコード

index.ts

```javascript
import { Command } from 'commander';

const program = new Command();

program
    .name('nodejs-sagyo')
    .description('Node.js ローカル作業')
    .version('0.0.1')

program.command('make')
    .description('JSONファイルを読み込んで、指定された種類のファイルを生成するプログラム')
    .option('-t, --type <string>', '生成するファイルの種類', 'sql')
    .argument('<fileName>', 'JSONファイル名')
    .action((fileName, options) => {
        if (fileName.endsWith('.json')) {
            console.error('ERROR 拡張子は不要です');
            process.exit(1);
        }
        const fileNameWithExt = fileName + '.json';

        switch (options.type) {
            case 'sql':
                //省略
                break;
            case 'json':
                //省略
                break;
            default:
                console.error('ERROR: 不明なファイルの種類です。');
                break;
        }
    });

program.command('check')
    .description('JSONファイルを読み込んで、チェックを行うプログラム')
    .argument('<fileName>', 'JSONファイル名')
    .action((fileName) => {
        if (fileName.endsWith('.json')) {
            console.error('ERROR 拡張子は不要です');
            process.exit(1);
        }
        const fileNameWithExt = fileName + '.json';
        //省略
    });

program.command('count')
    .description('SQLカウントクエリー文を作成する')
    .action(() => {
        //省略
    });

program.command('delete')
    .description('SQL削除クエリー文を作成する')
    .action(() => {
        //省略
    });

program.parse(process.argv);
```

## 3. コード実行

```bash
bun run index.ts check {01,02,03...}
bun run index.ts make --type sql {01,02,03...}
bun run index.ts make --type json {01,02,03...}
bun run index.ts count
bun run index.ts delete
```
