# winget とは

作成日 2021/06/05

## リリース記事を発見する

[日本語環境でも利用可能になった「Windows Package Manager 1\.0」がリリース \| TECH\+](https://news.mynavi.jp/article/20210527-1894790/)

> Microsoftは米国時間2021年5月26日、「Windows Package Manager(以下、winget)」のバージョン1.0をリリースしたことを明らかにした。wingetはWindows 10用パッケージ管理システムとして1年前の2020年5月から開発に取り組み、同社の開発者向けカンファレンス「Build 2021」に合わせて、当初の予定どおりバージョン1.0に至っている。

## 本家サイト = GitHub リポジトリ だった

[microsoft/winget\-cli: Windows Package Manager CLI \(aka winget\)](https://github.com/microsoft/winget-cli/)

## どうやってインストールするのか

Microsoft Store で配布されている「アプリ インストーラー」の一部になっている

これをインストールしてみたが、PowerShell で `winget` コマンドを叩いても、そんなプログラムは知らないと言われてしまった

### 手動でインストールしてみる

[Releases · microsoft/winget\-cli](https://github.com/microsoft/winget-cli/releases)

`Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.appxbundle` (17.5MB) をダウンロードする

ダブルクリックすると「アプリ インストーラー」が更新された

再び PowerShell で `winget` コマンドを叩くと、ヘルプが表示された

```bash
winget list
# => 設定 ＞ アプリと機能 で表示される一覧のテキスト版が表示された
```
