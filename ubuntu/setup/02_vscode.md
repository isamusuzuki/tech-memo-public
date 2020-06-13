# Visual Studio Code を使えるようにする

作成日 2020/06/13、更新日 2020/06/13

## Visual Studio Code をインストールする

[Visual Studio Code \- Code Editing\. Redefined](https://code.visualstudio.com/)

deb パッケージをダウンロードする ＞ code_1.46.0-15917880013_amd64.deb (59.7MB)

```bash
cd ~/Downloads
sudo apt install ./code_1.46.0-15917880013_amd64.deb
```

## フォント設定を変更する

自分の好みは、Robot Mono と Noto Sans Mono CJK JP を組み合わせ

### Roboto Mono をインストールする

[Roboto Mono \- Google Fonts](https://fonts.google.com/specimen/Roboto+Mono)

ページ右上 ＞ Download family ＞ Roboto_Mono.zip (659KB) を入手

解凍すると、10 種類の ttf フォントが登場する\
それらを `.fonts` フォルダにコピーする

```bash
cd ~
mkdir .fonts

mv Downloads/*.ttf .fonts

fc-cache

fc-list | grep "Roboto Mono"
```

### Visual Studio Code の設定を変更する

vscode > settings > Editor: Font Family

```text
'Roboto Mono','Noto Sans Mono CJK JP'
```

vscode を再起動する
