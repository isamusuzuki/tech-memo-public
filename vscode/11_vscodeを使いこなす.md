# Visual Studio Code を使いこなす

作成日 2020/01/29、更新日 2020/01/31

## 01. キーボードショートカット

- `Ctrl + ,` ... settings ページを開く
- `Ctrl + b` ... サイドバーを非表示にする／表示する
- `Ctrl + p` ... 最近開いたファイルの候補を表示する
- `Ctrl + g` ... 移動先の行数を指定する
- `Ctrl + Shift + p` ... コマンドパレットを表示する
- `Ctrl + Shift + e` ... エクスプローラーを表示する
- `Ctrl + Shift + f` ... 検索を表示する
- `Ctrl + Shift + g` ... SCM を表示する
- `Ctrl + Shift + Enter` ... ターミナルを表示する
- `Ctrl + Alt + 下矢印` ... マルチカーソルを下方向に展開する
- `Shift + Alt + 下矢印` ... カーソル行を下にコピーする
- `Ctrl + k , v` ... プレビューページをサイド・バイ・サイドで開く
- `Ctrl + k , f` ... フォルダを閉じる

## 02. ターミナルを Git Bash に変更する

- 左下歯車アイコン ＞ settings ＞ settings ページ
- `terminal integrated shell windows`を検索
- `Edit in settings.json`リンクをクリック
- `settings.json`ファイルが開かれる（ユーザーレベル）
- 以下を書き込む

```json=
{
  "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
}
```

## 03. ターミナルを表示するキーボードショートカットを変更する

- 左下歯車アイコン ＞ Keyboard Shortcuts ＞ Keyboard Shortcuts ページ
- `Toggle Integrated Terminal`を検索
- 見つけた項目をクリックするとモーダルが登場
- 新しいキー設定をタイプする。今回は `Ctrl + Shift + Enter`を入れる

## 04. HTML ファイルは Prettier にフォーマットさせない

.vscode/settings.json

```json
{
  "prettier.disableLanguages": ["html"],
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  }
}
```

## 05. HTML 終了タグに自動でマルチカーソルを当てさせない

この機能は November 2019 (version 1.41)で導入された新機能で、HTML mirror cursor という

[Visual Studio Code November 2019](https://code.visualstudio.com/updates/v1_41)

> HTML mirror cursor in tags - Automatic multi-cursor in matching HTML tags.

左下歯車アイコン ＞ Settings ＞ "html mirror"を検索する
＞ HTML: Mirror Cursor On Matching Tag
＞ チェックボックスを外す

## 06. 特定のファイルをエクスプローラーに表示させない

設定ページ ＞ `file exclude`を検索

`Files: Exclude`コーナーで、`**/desktop.ini`パターンを追加する

## 07. シフト JIS のファイルを開く

右下の `UTF-8` と書かれた部分をクリックして、`Reopen with Encoding` をクリックすると、エンコードが選択できる

文字コードの自動判別機能がデフォルトで `OFF` になっているので、設定を変更する

設定ページ ＞ `Files: Auto Guess Encoding`を検索 ＞ チェックを入れる

## 08. 拡張子ごとの関連付けを変更する

`spec.json.txt`というファイルは、JSON 扱いにする

```json
{
  "files.associations": {
    "*.json.txt": "json"
  }
}
```

## 09. 設定ファイルで使える定義済みの変数

```bash=
${workspaceFolder}
```

[Visual Studio Code Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)

## 10. 設定ファイルの格納場所

- Windows 10 ... `C:\Users\YOUR-NAME\AppData\Roaming\Code\User`
- Linux ... `$HOME/.config/Code/User`

## 11. 拡張子に基づいたおすすめをポップアップさせない

```json=
{
  "extensions.ignoreRecommendations": true
}
```
