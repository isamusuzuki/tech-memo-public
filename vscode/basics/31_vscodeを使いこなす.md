# vscode を使いこなす

作成日 2020/01/29、更新日 2020/04/02

## 03. HTML 終了タグに自動でマルチカーソルを当てさせない

この機能は November 2019 (version 1.41)で導入された新機能で、HTML mirror cursor という

[Visual Studio Code November 2019](https://code.visualstudio.com/updates/v1_41)

> HTML mirror cursor in tags - Automatic multi-cursor in matching HTML tags.

左下歯車アイコン ＞ Settings ＞ "html mirror"を検索する
＞ HTML: Mirror Cursor On Matching Tag
＞ チェックボックスを外す

## 04. 特定のファイルをエクスプローラーに表示させない

設定ページ ＞ `file exclude`を検索

`Files: Exclude`コーナーで、`**/desktop.ini`パターンを追加する

## 05. シフト JIS のファイルを開く

右下の `UTF-8` と書かれた部分をクリックして、`Reopen with Encoding` をクリックすると、エンコードが選択できる

文字コードの自動判別機能がデフォルトで `OFF` になっているので、設定を変更する

設定ページ ＞ `Files: Auto Guess Encoding`を検索 ＞ チェックを入れる

## 06. 拡張子ごとの関連付けを変更する

`spec.json.txt`というファイルは、JSON 扱いにする

```json
{
  "files.associations": {
    "*.json.txt": "json"
  }
}
```

## 07. 設定ファイルの格納場所

- Windows ... `C:\Users\YOUR-NAME\AppData\Roaming\Code\User`
- Linux ... `$HOME/.config/Code/User`

## 08. 拡張子に基づいたおすすめをポップアップさせない

```json
{
  "extensions.ignoreRecommendations": true
}
```
