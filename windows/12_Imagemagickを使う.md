# Imagemagick を使う

作成日 2019/11/22

## 01. Windows 10 に ImageMagick をインストールする

Chocolatey を使う => `choco install imagemagick.tool`

インストール先 => `C:\ProgramData\chocolatey\bin`

### 実行ファイル名の確認

1. compare.exe
1. composite.exe
1. conjure.exe
1. convert.exe
1. identify.exe
1. magick.exe
1. mogirify.exe
1. montage.exe
1. stream.exe

### `convert`コマンドだけ使えない

```bash
which convert
#=> /c/WINDOWS/system32/convert
```

これだけ PATH の順番により、Windows 標準アプリに負けている

=> `magick`コマンドを使うことで問題解決

### 日本語フォントとして Google Fonts を調達する

公式サイト => [https://fonts.google.com/](https://fonts.google.com/)

Noto Sans JP 紹介ページ => [https://fonts.google.com/specimen/Noto+Sans+JP](https://fonts.google.com/specimen/Noto+Sans+JP)

右上に SELECT THIS FONT があって、クリックすると、モーダルが登場し、その右上に、ダウンロードボタンがある

ダウンロードボタンをクリックすると、`Noto_Sans_JP.zip` (21.9MB) が入手できる

解凍した中身

1. NotoSansJP-Black.otf
1. NotoSansJP-Bold.otf
1. NotoSansJP-Light.otf
1. NotoSansJP-Medium.otf
1. NotoSansJP-Regular.otf
1. NotoSansJP-Thin.otf
1. OFL.txt

## 02. ImageMagick の公式ドキュメントを読む

### コマンドラインのオプション一覧

[ImageMagick \- Command\-line Options](https://www.imagemagick.org/script/command-line-options.php)

#### よく使うオプション

- `-annotate`
- `-compose`
- `-composite`
- `-crop`
- `-fill`
- `-geometry`
- `-gravity`
- `-pointsize`
- `-resize`

### Examples of ImageMagick Usage

[ImageMagick v6 Examples](https://www.imagemagick.org/Usage/)

### Practical Examples

- [Text to Image Handling](https://www.imagemagick.org/Usage/text/)
- [Compound Font Efffectc](https://www.imagemagick.org/Usage/fonts/)
- [Montage, Arrays of Images](https://www.imagemagick.org/Usage/montage/)
- [Layers of Multiple Images](https://www.imagemagick.org/Usage/layers/)

### Basic Techniques

- [Cutting and Bordering](https://www.imagemagick.org/Usage/crop/)

## 03. ImageMagick の基本的テクニック

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

中央に配置した商品画像から 10 ピクセルだけ下げたところに、\
説明文を挿入するには？ \
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

## 04. Photoshop が読めない JPEG 画像を探す

### 問題発生

同僚が、新規の E コマースサイトに商品画像を ZIP にまとめて FTP アップロードしていたところ、\
サーバーから蹴られる事態が発生 \
原因となる画像は、ブラウザやペイントなどでは問題なく表示できるのに、\
フォトショップで開くとエラーになる

![photoshop_error.jpg](https://i.imgur.com/rtxi6ah.jpg)

### 試行錯誤 1

まずは問題点を指摘できる方法を探る

```bash
identify -verbose temp/sample-06.jpg
# => identify.exe: Corrupt JPEG data: 66475 extraneous bytes before marker 0xd9
# => `temp/sample-06.jpg' @ warning/jpeg.c/JPEGWarningHandler/399.
```

ImageMagick でエラー文が表示されることがわかった \
これを Python のメソッドに組み込んだ

```python
from subprocess import getoutput

filename = 'sample-05.jpg'
cmd = f'identify -verbose temp/{filename}'
result = getoutput(cmd)
is_corrupt = 'Corrupt JPEG data:' in result

print(is_corrupt)
```

## 試行錯誤 2

既存の 1 万 6 千個のファイルを全部チェックする必要があった \
いったんローカルにコピーして、 \
連続して ImageMagick にチェックさせることにしたが、 \
途中で getoutput に文字コード変換エラーが起こる \
`try...except` で逃げた

```python
import os
from subprocess import getoutput

result_all = ''
for filename in os.listdir('temp'):
    print(filename)
    cmd = f'identify -verbose temp/{filename}'
    is_corrupt = False
    try:
        result = getoutput(cmd)
        if 'Corrupt JPEG data:' in result:
            is_corrupt = True
    except UnicodeDecodeError as e:
        is_corrupt = True
    finally:
        result_all += f'{filename} corrupt? => {is_corrupt}\n'

with open('result.txt', mode='w', encoding='utf-8') as f:
    f.write(result_all)

print('done')
```
