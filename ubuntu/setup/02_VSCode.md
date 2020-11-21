# Visual Studio Code

作成日 2020/06/13、更新日 2020/10/16

## 01. Visual Studio Code をインストールする

Ubuntu Software からインストールしてはいけない。日本語が入力できない問題がある

本家サイトから deb パッケージをダウンロードする

[Visual Studio Code \- Code Editing\. Redefined](https://code.visualstudio.com/)

```bash
cd ~/Downloads
sudo apt install ./code_1.50.1-1602600906_amd64.deb
```

## 02. フォント設定を変更する

自分の好みは、Robot Mono と Noto Sans Mono CJK JP を組み合わせである

### Roboto Mono をインストールする

[Roboto Mono \- Google Fonts](https://fonts.google.com/specimen/Roboto+Mono)

ページ右上 ＞ Download family ＞ Roboto_Mono.zip を入手

解凍すると、14 種類の ttf フォントが登場する\
それらを `.fonts` フォルダにコピーする

```bash
cd ~
mkdir .fonts
mv Downloads/*.ttf .fonts
fc-cache
fc-list | grep "Roboto Mono"
```

### Visual Studio Code の設定を変更する

※ Settings Sync（設定同期）をオンにしている場合は、以下の作業は不要

左下歯車アイコン > Settings > Editor: Font Family

```text
'Roboto Mono','Noto Sans Mono CJK JP'
```

VSCode を再起動する
