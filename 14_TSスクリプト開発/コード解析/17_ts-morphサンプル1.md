# ts-morphサンプルコード1

作成日 2025/12/11

```javascript
import { Project } from 'ts-morph';

// ts-morphに対象のtypescriptファイルを読ませる
const project = new Project();
const sourceFile = project.addSourceFileAtPath(tsFilepath);

// すべてのインポート宣言を取得
const importDeclarations = sourceFile.getImportDeclarations();
if (importDeclarations.length == 0) {
    throw new Error('インポート宣言が見つかりません。');
}
logger.info(`インポート宣言の数: ${importDeclarations.length}`);

// 最後のインポート宣言を特定
const lastImportDeclaration = importDeclarations[importDeclarations.length - 1];
if (!lastImportDeclaration) {
    throw new Error('最後のインポート行が見つかりません。');
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
