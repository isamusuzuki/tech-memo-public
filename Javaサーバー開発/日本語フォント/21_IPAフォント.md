# IPA フォント

作成日 2025/03/07

## 1. IPAex フォントと IPA フォントの違い

参照サイト => [IPAex フォントおよび IPA フォントについて](https://moji.or.jp/ipafont/)

1. 「IPAex フォント」は、和文文字（仮名や漢字など）は固定幅、欧文文字は文字幅に合わせた変動幅を基本とした実装を行い、日本語文書作成の利便性の向上を目指したフォントです。
1. 欧文文字、和文文字ともに固定幅の「IPA 明朝」と「IPA ゴシック」
1. 欧文文字、和文文字ともに変動幅の「IPA P 明朝」と「IPA P ゴシック」

## 2. 入手方法

- [IPA フォント Ver.003.03](https://moji.or.jp/ipafont/ipa00303/)
- IPA 明朝(IPA 明朝・IPA P 明朝 2 書体パック「TTC ファイル」)(Ver.003.03)をダウンロードする
- `IPAMTTC00303.zip`ファイルを入手して解凍する
- `ipam.ttc` (8,095 KB)を入手する

ファイルのプロパティ＞詳細を確認する

| Key                  | Value                                 |
| -------------------- | ------------------------------------- |
| タイトル             | IPA 明朝; IPA P 明朝                  |
| 種類                 | TrueType コレクションフォントファイル |
| バージョン           | 003.03                                |
| フォント埋め込み可能 | インストール可能                      |

## 3. ttc ファイルはそのままでは使えない問題

- コレクションフォントファイルには複数のフォントが埋め込まれている
- ttc ファイルを単一のフォントファイルと同じように使うとエラーが発生する

```text
Font 'ipam.ttc' with 'Identity-H' is not recognized.
Font 'ipam.ttc' with 'Identity-H' is not recognized.
```

BaseFont.class のコメントにコレクションからのフォントの取り出し方が書いてあった

```text
Fonts in TrueType collections are addressed by index such as "msgothic.ttc,1". This would get the second font (indexes start at 0), in this case "MS PGothic".
```

- `ipam.ttc.0` ... IPA 明朝フォントが取り出せた
- `ipam.ttc,1` ... IPA P 明朝フォントが取り出せた

### 3a. 注意点

- フォントファイルのフルパスを求めるときに指定数字を入れるとエラーになる
- フルパスを求めてから指定数字を入れる

```java
Path path = Path.of(JapaneseFont.class.getClassLoader().getResource(filename).toURI());
String fontPath = path.toString();
if (collectionOrder >= 0) {
    fontPath = fontPath + "," + collectionOrder;
}
BaseFont baseFont = BaseFont.createFont(
    fontPath, BaseFont.IDENTITY_H, BaseFont.EMBEDDED);
```
