# Record型

作成日 2025/07/29

参照記事 => [Record<Keys, Type>](https://typescriptbook.jp/reference/type-reuse/utility-types/record)

> `Record<Keys, Type>`はプロパティのキーが`Keys`であり、プロパティの値が`Type`であるオブジェクトの型を作るユーティリティ型です。

```javascript
// 各タスクのsourceFilenameを集計して重複をチェックする
const filename_list: Record<string, number> = {};
for (const task of tasks) {
    const filename = task["sourceFilename"];
    if (filename_list[filename]) {
        filename_list[filename]++;
    } else {
        filename_list[filename] = 1;
    }
}

const duplicates = Object.entries(filename_list)
    .filter(([_, count]) => count > 1);
```
