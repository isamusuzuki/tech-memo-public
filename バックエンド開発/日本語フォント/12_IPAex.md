# IPAex フォント

作成日 2025/01/21

## 入手方法

- [IPAex フォントおよび IPA フォントについて](https://moji.or.jp/ipafont/)
- [IPAex フォント Ver.004.01](https://moji.or.jp/ipafont/ipaex00401/)
- IPAex ゴシック(Ver.004.01)をダウンロードする
- `ipaexg00401.zip`ファイルを入手して解凍する
- `ipaexg.ttf` (5.9MB)を入手する

## 使い方

使いたい Java プロジェクトの resource フォルダに置く

```java
Path path = Path.of(Nihongo.class.getClassLoader().getResource("ipaexg.ttf").toURI());
logger.info(path.toString());
```
