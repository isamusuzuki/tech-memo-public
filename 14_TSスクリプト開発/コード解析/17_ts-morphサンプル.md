# ts-morphサンプルコード

作成日 2025/12/11、更新日 2025/12/18

## 1. インポート宣言の最後に新しいインポート宣言を追加する

```javascript
import { Project } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// すべてのインポート宣言を取得する
const importDeclarations = sourceFile.getImportDeclarations();
if (importDeclarations.length == 0) {
    throw new Error(`${name}: インポート宣言が見つかりません`);
}

// 最後のインポート宣言を特定する
const lastImportDeclaration = importDeclarations[importDeclarations.length - 1];
if (!lastImportDeclaration) {
    throw new Error(`${name}: 最後のインポート行が見つかりません`);
}

// 最後のインポート宣言の次に新しいインポート宣言を追加する
sourceFile.insertImportDeclaration(lastImportDeclaration.getChildIndex() + 1, {
    namedImports: ['log'],
    moduleSpecifier: 'share/src',
});

// 変更を保存する
await sourceFile.save();
logger.info(`${name}: added import declaration successfully.`);
```

- `.getImportDeclarations()`        ... 全てのインポート宣言を取得
- `.getChildIndex()`                ... 親から見た自分（子）のインデックスを取得
- `.insertImportDeclaration(0, {})` ... 新しいインポート宣言を追加
- `{namedImports:L ['xxx'], moduleSpecifier: 'zzz'}` ... インポート宣言を構築

## 2. 特定の変数宣言の内部にlog.info文を追加する

```javascript
import { Project, SyntaxKind } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// 特定の変数を取得する
const targetDeclaration = sourceFile.getVariableDeclaration(eventName);
if (!targetDeclaration) {
    // 変数が見つからない場合は関数宣言を試す
}

// 変数の初期化子（アロー関数）を取得する
const initializer = targetDeclaration.getInitializer();
if (!initializer) {
    throw new Error(`${name}: ${eventName}の初期化子が見つかりません`);
}

// 変数内部の先頭にlog.info('button clicked');を追加する
if (initializer.getKind() === SyntaxKind.ArrowFunction) {
    const arrowFunction = initializer.asKind(SyntaxKind.ArrowFunction);
    if (arrowFunction) {
        const body = arrowFunction.getBody();
        if (body && body.getKind() === SyntaxKind.Block) {
            const blockBody = body.asKind(SyntaxKind.Block);
            if (blockBody) {
                blockBody.insertStatements(0, 'log.info("button clicked");');
            }
        }
    }
}

// 変更を保存する
await sourceFile.save();
logger.info(`${name}: added log info successfully.`);
```

- `.getVariableDeclaration(xxx)` ... 名前で変数宣言を取得
- `.getInitializer()` ... 初期化子を取得（変数宣言なので、そこは初期値のポジション）
- `.getKind()`        ... 種類を取得
- `SyntaxKind.ArrowFunction`     ... （種類）アロー関数
- `.asKind()`         ... 種類の変換
- `.getBody()`        ... 関数のボディを取得
- `SyntaxKind.Block`  ... （種類）ブロック
- `.insertStatements(0,'xxx')`   ... ステートメントを追加

## 3. 特定の関数宣言の内部にlog.info文を追加する

```javascript
import { Project, SyntaxKind } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// 特定の関数宣言を取得する
const targetFunction = sourceFile.getFunctionOrThrow(eventName);
const functionBody = targetFunction.getBodyOrThrow();
// 関数宣言内部の先頭にlog.info文を追加する
if (!functionBody || functionBody.getKind() !== SyntaxKind.Block) {
    throw new Error(`${name}: ${eventName}関数の本体が見つかりません`);
} else {
    const block = functionBody.asKind(SyntaxKind.Block);
    if (!block) {
        throw new Error(`${name}: ${eventName}関数の本体がブロックではありません`);
    } else {
        // ここでブロックに対して操作を行う
        block.insertStatements(0, 'log.info("button clicked");');
    }
}

// 変更を保存する
await sourceFile.save();
logger.info(`${name}: added log info successfully.`);
```

- `.getFunctionOrThrow(xxx)` ... 名前で関数宣言を取得
- `.getBodyOrThrow()` ... 関数宣言のボディを取得

## 4. onMounted()にlog.info文を追加する

```javascript
import { Project, SyntaxKind } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// onMounted()を取得する
const onMountedCalls = sourceFile.getDescendantsOfKind(SyntaxKind.CallExpression).filter((callExpr) => {
    const expression = callExpr.getExpression();
    return expression.getText() === 'onMounted';
});

if (onMountedCalls.length === 0) {
    // onMounted()が存在しない場合は新規追加する
    // インポート宣言の最後にonMountedのインポート宣言を追加するコード（略）
    // ソースファイルの最終行にonMounted()を追加するコード（略）
} else {
    // 最初のonMounted()の中にlog.info文を追加する
    const firstCall = onMountedCalls[0];
    const args = firstCall!.getArguments();
    if (args.length === 0) {
        throw new Error(`${name}: onMounted()に引数がありません。`);
    }
    const firstArg = args[0];
    if (firstArg && firstArg.getKind() !== SyntaxKind.ArrowFunction && firstArg.getKind() !== SyntaxKind.FunctionExpression) {
        throw new Error(`${name}: onMounted()の最初の引数が関数ではありません。`);
    }
    const func = firstArg!.asKind(SyntaxKind.ArrowFunction) || firstArg!.asKind(SyntaxKind.FunctionExpression);
    const body = func!.getBody();
    if (body && body.getKind() === SyntaxKind.Block) {
        const blockBody = body.asKind(SyntaxKind.Block);
        if (blockBody) {
            const message = `「${screenName}」ページが表示されました`;
            blockBody.insertStatements(0, `log.info('${message}');`);
        }
    }
}

// 変更を保存する
await sourceFile.save();
logger.info(`${name}: added log info successfully.`);
```

- `.getDescendantsOfKind(xxx)` ... 種類で子ノードを取得
- `SyntaxKind.CallExpression` ... （種類）関数呼び出し
- `.getExpression()`          ... 式の取得
- `.getArguments()`           ... 引数の取得

## 5. 最終行にonMounted()を追加する

```javascript
import { Project } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

const message = `「${screenName}」ページが表示されました`;
sourceFile.addStatements(`
onMounted(() => {
    log.info('${message}');
});
`);

// 変更を保存する
await sourceFile.save();
logger.info(`${name}: added log info successfully.`);
```

- `.addStatements(xxx)` ... 文を追加する
