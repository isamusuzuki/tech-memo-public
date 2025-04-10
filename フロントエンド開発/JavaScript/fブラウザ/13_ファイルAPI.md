# ファイルAPI

作成日 2025/04/10

## 1. [ファイルAPIの概念と使い方](https://developer.mozilla.org/ja/docs/Web/API/File_API)

- ファイルAPI は、ウェブアプリケーションがファイルとそのコンテンツにアクセスできるようにするためのもの
- ウェブアプリケーションは、`file`型の`<input>`要素を使用するか、ドラッグ＆ドロップを介するかのどちらかでファイルにアクセスすることができるようになる
- 利用可能になった一連のファイルは`FileList`オブジェクトとして表され、ウェブアプリケーションが個々の`File`オブジェクトを取得することができるようになっている
- `File`オブジェクトを`FileReader`オブジェクトに渡すことで、ファイルの内容にアクセスできる
- `FileReader`インターフェイスは非同期だが、ウェブワーカーでのみ利用できる同期バージョンが`FileReaderSync`インターフェイスで提供されている

## 2. [Blobインターフェイス](https://developer.mozilla.org/ja/docs/Web/API/Blob)

[Blob()コンストラクター](https://developer.mozilla.org/ja/docs/Web/API/Blob/Blob)

構文

```javascript
new Blob(blobParts)
new Blob(blobParts, options)
```

引数

- blobParts ... 反復可能オブジェクト、例えばArrayなど（省略可）
- options ... type, endingsというプロパティを持つオブジェクト（省略可）

返り値

- 指定されたデータを含むBlobオブジェクト

例

```javascript
const blobParts = ['<q id="a"><span id="b">hey!</span></q>']; // 単一の文字列からなる配列
const blob = new Blob(blobParts, { type: "text/html" }); // blob
```
