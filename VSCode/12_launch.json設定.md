# launch.json 設定

作成日 2025/02/11

## ユーザーガイド（英語）を読む

[Debugging](https://code.visualstudio.com/Docs/editor/debugging)

## 簡単な設定例

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/server.js"
    }
  ]
}
```

configurations の項目解説

| Name    | Comment                                 |
| ------- | --------------------------------------- |
| type    | 使用する Debugger                       |
| request | デバッグ実行のモード (launch or attach) |
| name    | ドロップダウンに登場する名前            |
| program | デバッグ対象のプログラムのパス          |
| args    | デバッグ実行時に渡される引数            |

定義済みの変数

| Name                         | Comment                            |
| ---------------------------- | ---------------------------------- |
| `${file}`                    | 現在開いているファイルのパス       |
| `${fileBasename}`            | 現在開いているファイルの名前       |
| `${workspaceFolder}`         | VS Code が開いているフォルダのパス |
| `${workspaceFolderBasename}` | VS Code が開いているフォルダの名前 |
