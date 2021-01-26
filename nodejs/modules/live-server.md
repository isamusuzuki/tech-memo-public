# live-server

作成日 2021/01/26

- 起動すると、自動的にブラウザを開く
- オプションなしで PATH を書けば、そこがドキュメントルートになる
- ドキュメントに変更があれば自動的にリロードする
- entry-file を設定すると、404 エラーのときも index.html を開く（SPA 対応）
- node_modules フォルダをマウントできる `--mount=/node_modules:./node_modules`

```json
{
  "scripts": {
    "dev": "live-server ./dist --entry-file=./dist/index.html"
  }
}
```
