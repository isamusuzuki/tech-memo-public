# ImageMagick

作成日 2020/05/24

## 01. ImageMagickとは

コマンドベースの画像編集ツール

公式トップ => [ImageMagick \- Convert, Edit, or Compose Bitmap Images](https://imagemagick.org/index.php)

インストール => `sudo apt install imagemagick -y`

## 02. ImageMagick を使う

バージョン6.9の段階では、まだmagickという統一コマンドはなく、\
複数のコマンドの集合体となっている

```bash
convert --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

# ファイルアプリで、Linuxファイルの中に新しいフォルダをつくり
# その中に、変換したい画像ファイルをコピーする
cd ~/Downloads/images

# 画像ファイルを一括でサイズ変換する
# 横幅のサイズだけを指定。縦横の比率は保たれる
mogrify -resize 200 *.jpg
```
