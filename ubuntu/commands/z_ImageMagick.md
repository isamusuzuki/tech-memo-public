# ImageMagick

作成日 2020/06/01、更新日 2020/06/30

## 01. ImageMagick とは

コマンドベースの画像編集ツール

公式トップ => [ImageMagick \- Convert, Edit, or Compose Bitmap Images](https://imagemagick.org/index.php)

インストール => `sudo apt install imagemagick -y`

公式トップには最新版は 7.0 だとあるが、実際に Ubuntu にインストールすると、\
6.9 がインストールされる。6.9 の段階では、まだ magick という統一コマンドはない

### 複数コマンドの集合体

```bash
compare --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

composite --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

conjure -version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

convert --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

identify --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

mogrify --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

montage --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101

stream --version
# => Version: ImageMagick 6.9.10-23 Q16 x86_64 20190101
```

## 02. ImageMagick の基本的な使い方

### 画像をリサイズする

```bash
# 縦横比を維持したまま、指定サイズに収まるようにリサイズする
convert -resize 640x480 original.jpg new.jpg

# 縦横比を無視して、指定サイズに収まるようにリサイズする
convert -resize 640x480! original.jpg new.jpg
```

### 画像の情報を取得する

```bash
identify original.jpg
```

### 白画像を生成する

```bash
convert -size 200x200 xc:white white.jpg
```

### 文章を挿入する

```bash
convert original.jpg \
-font "./RobotoMono-Bold.ttf" \
-annotate +350+50 "Taro Okamoto" new.jpg
```

中央に配置した商品画像から 10 ピクセルだけ下げたところに、説明文を挿入するには？ \
`-gravity`オプションを north にセットし、 商品画像のエンドに+10 する

```bash
convert {INPUT} -font {FONT} \
-fill gray -pointsize 24 \
-gravity north \
-annotate +0+{FONT_POSITION} "{TEXT}" {OUTPUT}
```

### 画像の一括サイズ変換

```bash
# 横幅のサイズだけを指定。縦横の比率は保たれる
mogrify -resize 200 *.jpg
```

### 画像を合成する

構文 1 \
`composite -gravity <position> -compose <operator> <上画像名> <下画像名> <生成画像名>`

構文 2 \
`convert <下画像名> <上画像名> -gravity <position> -compose <operator> -composite <生成画像名>`

`-compose`オプションで使用できる引数 \
`src-over` ... the source is composited over the destination

`-gravity`オプションの代わりに、`-geometry`オプションを使えば、正確に位置を指定できる

構文 1 の変形 \
`composite -geometry <100x100+62+50> -compose <operator> <上画像名> <下画像名> <生成画像名>`

位置指定方法 \
`<width>x<height>+<xoffset>+<yoffset>`

### 複数の画像を合成する

```bash
convert -size 100x100 xc:skyblue comp_resize.gif
composite -geometry 40x40+5+10  balloon.gif comp_resize.gif comp_resize.gif
composite -geometry      +35+30 medical.gif comp_resize.gif comp_resize.gif
composite -geometry 24x24+62+50 present.gif comp_resize.gif comp_resize.gif
composite -geometry 16x16+10+55 shading.gif comp_resize.gif comp_resize.gif
```

### 画像を自動で切り抜きする

```bash
# 基本形
convert --trim org.png new.png

# 背景を削っていくので、商品にズームアップしたような画像になる
convert -define trim:percent-background=5% -trim org.png new.png

# 背景画像の判断をゆるくすると、きっちり切り抜きできる
convert -fuzz 50% -trim org.png new.png
```

### PNG 画像を JPG 画像に変換するときに、透過部分を白にする

```bash
magick original.png
-resize 640x480 -extent 640x480
new.jpg
```

### 縦横比を維持しつつサイズを変更し、余白は白で塗りつぶす

```bash
convert c1.png -resize 1000x1000 -gravity center \
-background white -extent 1000x1000 c1a.png
```
