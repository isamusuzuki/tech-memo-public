# ImageMagick

作成日 2020/06/01

## 01. ImageMagickとは

コマンドベースの画像編集ツール

公式トップ => [ImageMagick \- Convert, Edit, or Compose Bitmap Images](https://imagemagick.org/index.php)

インストール => `sudo apt install imagemagick -y`

マニュアルは、v7.0と先に行ってしまっていて参考にならない

### 複数コマンドの集合体

バージョン6.9の段階では、まだmagickという統一コマンドはない

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

## 02. ImageMagick のノウハウ

### ImageMagick の原理原則

オプションの順番がとても大事 \
とくに設定オプションは、実行オプションの後に置くと、\
先に実行オプションが走るため、\
設定がまったく反映されないので要注意

```bash
# 誤った例
magick -trim -fuzz 50% orginal.png new.png

# 正しい例
magick -fuzz 50% -trim orginal.png new.png
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

### 白画像を生成する

```bash
cd ~/Desktop
magick -size 200x200 xc:white white.jpg
```

### 文章を挿入する

```bash
magick test.jpg \
-font "fonts/NotoSansJP-Regular.otf" \
-annotate +50+160 \
"岡本太郎" test.jpg
```

中央に配置した商品画像から 10 ピクセルだけ下げたところに、説明文を挿入するには？ \
`-gravity`オプションを north にセットし、 商品画像のエンドに+10 する

```bash
magick {INPUT} -font {FONT} \
-fill gray -pointsize 24
-gravity north -annotate +0+{FONT_POSITION} "{TEXT}" {OUTPUT}
```

### `-trim`オプションを使って、画像を自動で切り抜きする

```bash
# 基本形
magick --trim org.png new.png

# 背景を削っていくので、商品にズームアップしたような画像になる
magick -define trim:percent-background=5% -trim org.png new.png

# 背景画像の判断をゆるくすると、きっちり切り抜きできる
magick -fuzz 50% -trim org.png new.png
```

### Identify コマンドで画像の情報を取得する

```python
from subprocess import getoutput

for color in spec['colors']:
    cmd = f'identify -format "%w,%h" temp/{color["filename"]}'
    size = getoutput(cmd).split(',')
    width = int(size[0])
    height = int(size[1])
```

### PNG 画像を JPG 画像に変換するときに、透過部分を白にする

```python
import subprocess

for color in spec['colors']:
    cmd = (
        f'magick images/{spec["item_code"]}/color/{color["filename"]} '
        f'-resize {width}x{height} -extent {width}x{height} '
        f'temp/{color["filename"].replace(".png", ".jpg")}'
    )
    proc = subprocess.Popen(cmd, shell=True)
    print(f'process {proc.pid} starts')
    proc.wait()
```

### 縦横比を維持しつつサイズを変更し、余白は白で塗りつぶす

```bash
magick c1.png -resize 1000x1000 -gravity center \
-background white -extent 1000x1000 ../temp/c1.png
```

### 画像の一括サイズ変換

```bash
# ファイルアプリで、Linuxファイルの中に新しいフォルダをつくり
# その中に、変換したい画像ファイルをコピーする
cd ~/Downloads/images

# 画像ファイルを一括でサイズ変換する
# 横幅のサイズだけを指定。縦横の比率は保たれる
mogrify -resize 200 *.jpg
```
