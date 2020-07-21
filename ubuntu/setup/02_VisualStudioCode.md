# Visual Studio Code

作成日 2020/06/13、更新日 2020/07/08

## Visual Studio Code をインストールする

Ubuntu Software からインストールしてはいけない。日本語が入力できない問題がある

本家サイトから deb パッケージをダウンロードする

```bash
cd ~/Downloads
sudo apt install ./code_1.46.1-1592428892_amd64.deb
```

[Download Visual Studio Code \- Mac, Linux, Windows](https://code.visualstudio.com/download)

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