# ts-morphサンプルコード

作成日 2025/12/11

## 1. インポート宣言の最後に新しいインポート宣言を追加する

```javascript
import { Project } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// すべてのインポート宣言を取得
const importDeclarations = sourceFile.getImportDeclarations();
if (importDeclarations.length == 0) {
    logger.error('インポート宣言が見つかりません。');
    return;
}
logger.info(`インポート宣言の数: ${importDeclarations.length}`);

// 最後のインポート宣言を特定
const lastImportDeclaration = importDeclarations[importDeclarations.length - 1];
if (!lastImportDeclaration) {
    logger.error('最後のインポート行が見つかりません。');
    return;
}
logger.info(`最後のインポート行: ${lastImportDeclaration.getText()}`);

// 最後のインポート宣言の次に新しいインポート宣言を追加
sourceFile.insertImportDeclaration(lastImportDeclaration.getChildIndex() + 1, {
    namedImports: ['log'],
    moduleSpecifier: 'share/src',
});

// 対象のTypeScriptファイルを更新
await sourceFile.save();
logger.info('インポート宣言を追加し、ファイルを更新しました');
```

## 2. 特定の変数の内部にconsole.log文を追加する

```javascript
import { Project, SyntaxKind } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// 特定の変数を取得する
const targetDeclaration = sourceFile.getVariableDeclaration('OnClickSearchButton');
if (!targetDeclaration) {
    logger.error('指定の変数が見つかりません');
    return;
}

// 変数の初期化子（アロー関数）を取得
const initializer = targetDeclaration.getInitializer();
if (!initializer) {
    logger.error('指定のメソッドの初期化子が見つかりません');
    return;
}

// 変数の先頭にconsole.log('button clicked');を追加する
if (initializer.getKind() === SyntaxKind.ArrowFunction) {
    const arrowFunction = initializer.asKind(SyntaxKind.ArrowFunction);
    if (arrowFunction) {
        const body = arrowFunction.getBody();
        if (body && body.getKind() === SyntaxKind.Block) {
            const blockBody = body.asKind(SyntaxKind.Block);
            if (blockBody) {
                blockBody.insertStatements(0, 'console.log("button clicked");');
                logger.info('指定の変数の先頭にconsole.log();を追加しました');
            }
        }
    }
}

// 対象のTypeScriptファイルを更新する
await sourceFile.save();
logger.info('特定の変数の内部にconsole.log文を追加し、ファイルを更新しました');
```

### ts-morphの使い方を理解する

- `.getVariableDeclaration(OnClickSearchButton)` ... 名前で変数宣言を取得
- `.getInitializer()` ... 初期化子を取得（あくまで変数宣言だから、そこは初期値のポジション）
- `.getKind()` ... 種類を取得
- `SyntaxKind.ArrowFunction` ... （種類）アロー関数
- `.asKind()` ... 種類の変換
- `.getBody()` ... アロー関数のボディパートを取得
- `SyntaxKind.Block` ... （種類）ブロック
- `.insertStatements(0,'console.log()')` ... ステートメントを追加
