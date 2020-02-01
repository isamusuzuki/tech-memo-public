# Jetson Nano Developer Kit をセットアップする

作成日 2019/08/19、更新日 2019/09/15

公式ドキュメント => [Getting Started With Jetson Nano Developer Kit](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)

参考記事 => [小型 AI コンピュータ NVIDIA Jetson Nano 開発者キットのセットアップ](https://qiita.com/tsutof/items/66e68c75b462c4faf1cb)

## 01. miscrSD カードに最新イメージを書き込む

### OS イメージファイルを入手する

[Jetson Download Center](https://developer.nvidia.com/embedded/downloads)

| Key          | Value                                   |
| ------------ | --------------------------------------- |
| Title        | Jetson Nano Developer Kit SD Card Image |
| Version      | JP 4.2.2                                |
| Release Date | 2019/08/26                              |

jetson-nano-sd-r32.2-2019-08-26.zip (5.0GB) をダウンロードする

解凍して、sd-blob-b01.img (5.3GB) を入手する

### SD Memory Card Formatter for Windows を入手する

[SD Memory Card Formatter for Windows Download](https://www.sdcard.org/downloads/formatter/eula_windows/)

SDCardFormatterv5_WinEN.zip (6.02MB) をダウンロードする

2 回解凍して、セットアッププログラムを実行すると、SD Card Formatter がインストールされる

プログラム起動 ＞ カードドライブを選択 ＞ クイックフォーマットを選択 ＞ ボリュームレベルは空のまま ＞ フォーマットボタン

### balenaEtcher を入手する

[balenaEtcher \- Home](https://www.balena.io/etcher/)

balenaEtcher-Setup-1.5.53.exe(139MB)を入手する

セットアッププログラムを実行すると、balenaEtcher がインストールされる

### balenaEtcher で OS イメージファイルを、microSD カードにコピーする

プログラム起動 ＞ Select image ＞ sd-blob-b01.img を選択 ＞ Flash! ボタン

## 02. ユーザーガイドを読む

PDF 形式で提供されている。最新版を入手するには、OS イメージと同じくダウンロードセンターに行く

| Key          | Value                                |
| ------------ | ------------------------------------ |
| Title        | Jetson Nano Developer Kit User Guide |
| Version      | 1.1                                  |
| Release Date | 2019/07/19                           |

### P.8 Power Guide

-   このキットは、2A が供給可能な 5V の電源を必要とする
-   箱から出した直後は、マイクロ USB 接続が有効になっている
-   本体モジュールは、動作に最低 4.75V を必要とする
-   もし、総負荷が 2A を超える予想ならば、J48 ヘッダーピンを接続する
-   J48 は、マイクロ USB 経由を無効にし、J25 電源ジャック経由の電源供給を有効にする
-   J25 は、5V-4A を供給する、深さ 9.5mm、内径 2.1mm、外径 5.5mm の正極プラグである
