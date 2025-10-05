# Java ファイルのフォーマッター

作成日 2025/01/27

"Language Support for Java by Red Hat" をフォーマッターに指定し、ファイルを保存するときに毎回フォーマットさせる

`.vscode/settings.json`

```json
{
  "[java]": {
    "editor.defaultFormatter": "redhat.java",
    "editor.formatOnSave": true
  }
}
```
