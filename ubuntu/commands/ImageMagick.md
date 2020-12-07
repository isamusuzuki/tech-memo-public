# ImageMagick

作成日 2020/12/07

## 01. ImageMagick とは

コマンドベースの画像編集ツール

公式トップ => [ImageMagick \- Convert, Edit, or Compose Bitmap Images](https://imagemagick.org/index.php)

### ImageMagick のインストール

apt コマンドでインストールすると、バージョン 6.9 が入ってしまう

6.9 は、まだ magick という統一コマンドがない

代わりに、公式ページからバイナリをダウンロードする

```bash
wget https://imagemagick.org/download/binaries/magick
chmod 755 magick

# PATHが通ったところに置く
sudo mv magick /usr/local/bin

magick --version
# => Version: ImageMagick 7.0.10-44 Q16 x86_64 2020-11-30 https://imagemagick.org
```

## 02. ImageMagick の基本的な使い方

### 画像をリサイズする

```bash
# 縦横比を維持したまま、指定サイズに収まるようにリサイズする
magick original.png -resize 100x150 new.png
# => オリジナルが正方形だったら、100x100にしかならない

# 縦横比を無視して、指定サイズに収まるようにリサイズする
magick original.png -resize 100x150! new.png
# => オリジナルが正方形でも、縦長の形に変更される
```

### 画像の情報を取得する

```bash
magick identify original.png
# => original.png PNG 128x128 128x128+0+0 8-bit sRGB 6611B 0.000u 0:00.000
```

### 白画像を生成する

```bash
magick -size 200x200 xc:white white.jpg
```

### 文章を挿入する

```bash
magick white.jpg -font "../fonts/YuGothM.ttc" -annotate +100+100 "岡本太郎" new.jpg
```

中央に配置した商品画像から 10 ピクセルだけ下げたところに、説明文を挿入する

`-gravity`オプションを north にセットし、 商品画像の高さに `+10` する。仮にそれが150だった場合

```bash
magick white.jpg -font "../fonts/YuGothM.ttc" -fill gray -pointsize 24 -gravity north -annotate +0+150 "岡本太郎" new.jpg
```

### 画像を合成する

```bash
magick composite -colorspace sRGB -geometry 100x100+50+50 andy.png white.jpg new.jpg
```

構文\
`magick composite -geometry <位置指定> <貼り付け画像名> <下地画像名> <生成画像名>`

位置指定方法\
`<width>x<height>+<xoffset>+<yoffset>`

### PNG 画像を JPG 画像に変換するときに、透過部分を白にする

```bash
magick andy.png -resize 200x200 -extent 200x200 andy.jpg
```

### 縦横比を維持しつつサイズを変更し、余白は灰色で塗りつぶす

```bash
magick andy.jpg -resize 290x290 -gravity center -background gray -extent 300x300 andy1.jpg
```

### 画像を自動で切り抜きする

```bash
# 基本形
magick new.jpg -fuzz 0% -trim new2.jpg

# 背景画像の判断をゆるくすると、きっちり切り抜きできる
magick new.jpg -fuzz 50% -trim new3.jpg

# 背景を削っていくので、商品にズームアップしたような画像になる
magick new.jpg -define trim:percent-background=5% -trim new4.jpg
```

### 画像の一括サイズ変換

```bash
# 横幅のサイズだけを指定。縦横の比率は保たれる
magick mogrify -resize 50 *.jpg
```
