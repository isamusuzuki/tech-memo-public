# 開発環境構築

作成日 2026/01/03

## 1. パソコン

WindowsでもMacでもどちらでも構わない。デスクトップでもノートブックでもどちらでも構わない

CPUは、それほど古くないもの（おそそ5年以内）ならばIntelでもAMDでもARMでもなんでも構わない

GPUも、それほど古くないもの（およそ5年以内）ならば内蔵でも外付けでもなんでも構わない

モニターは大きければ大きいほうがいい。モニターの枚数も多いほうがいい。メモリも多いほうがいい

モニターが1枚しかなく、メモリも8GBしかなくても、とりあえず、自分専用に使えるパソコンが必要

## 2. ターミナル

ターミナルのメリット【その１】コマンドをコピペできるので、操作方法をメモしやすい。

ターミナルのメリット【その２】サーバーにSSH接続して、操作ができる

正確に記述できる => 記述をまとめるとスクリプトになる => 自動化への道

コマンドに慣れてくると、Dockerfile（コンテナ設定）、Ansible,Terraform（サーバー管理ツール）もだんだん理解できるようになる。ターミナルは自分を鍛える養成ギブス

Windowsユーザーは、[Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701?hl=ja-JP&gl=JP)をインストールする

Windows Terminalのメリット【その１】マウスの範囲選択でコピぺができる。これ便利

Windows Terminalのメリット【その２】タブでPowerShell,WSL,Git Bash等のターミナルを切り替えられる

## 3. Linux

インターネット上のサーバーのほとんどはLinux。Dockerで立ち上げるコンテナはすべてLinux

ディストリビューションはDebian(Ubuntu)。Docker Hubで提供されているオフィシャルコンテナのほとんどが、DebianもしくはAlpine Linuxをベースにしている。Alpine Linuxは、極限まで軽量化を実現するため「フツー」と大きく異なっているので、初心者は手を出さないほうがいい

Windowsユーザーは、SL(Windows Subsystem for Linux)をインストールする。ProエディションでなくてもHomeエディションであっても問題なくインストールできる。完全なLinuxが手に入る

WSLをインストールするときに参照できる => [WSLインストール](https://github.com/isamusuzuki/tech-memo-public/blob/main/43_OS/Windows/12_WSL%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB.md)

Linuxを勉強するときに参照できる => [Ubuntu/basics](https://github.com/isamusuzuki/tech-memo-public/tree/main/43_OS/Ubuntu/basics)

Linuxコマンドを勉強するときに参照できる => [Ubuntu/commands](https://github.com/isamusuzuki/tech-memo-public/tree/main/43_OS/Ubuntu/commands)

シェルスクリプトを勉強するときに参照できる => [Ubuntu/shell-scripts](https://github.com/isamusuzuki/tech-memo-public/tree/main/43_OS/Ubuntu/shell-scripts)

## 4. Git/GitHub

Windowsユーザーは、Windows Terminalの次に[Git](https://git-scm.com/)をインストールする

Macユーザーは、Homebrew経由でGitをインストールする => [Homebrewとは](https://github.com/isamusuzuki/tech-memo-public/blob/main/43_OS/MacOS/14_Homebrew.md)

GitHubは、無料でアカウントを作成でき、無料アカウントでもプライベートリポジトリを無制限に作成できる => [GitHub](https://github.com/)

新しい開発を開始する前に、GitHubにプライベートリポジトリを作成する。できたばかりの空のリポジトリを自分のパソコンにクローンしてから、開発が始まる。ちょっと開発しては、コードをコミットする。何回かコミットして一区切りついたら、GitHubにプッシュする。複数人で開発するようになったら、ブランチを切る。GitHub Actionsを利用して、テスト・デプロイを実現する。Git/GitHubを使ってコードを育てていくのが、開発のキホン

GitHubとの連携には、SSH接続を使うのが便利 => [新しい PC で GitHub を使えるように連携する](https://github.com/isamusuzuki/tech-memo-public/blob/main/41_%E9%96%8B%E7%99%BA%E3%83%84%E3%83%BC%E3%83%AB/Git/_%E6%96%B0PC%E3%81%A7GitHub%E9%80%A3%E6%90%BA.md)

## 5. 開発コンテナ

PythonやNode.jsといったプログラミング言語・開発ツールを、直接OSにインストールすることなく、リポジトリごとに準備できる。開発のためにコンテナを構築して、その中で開発を行う

開発コンテナのメリット【その１】簡単に何度でも作り直せる。OSは汚れない

開発コンテナのメリット【その２】VSCodeの設定・インストールする機能拡張も記述できる

開発コンテナのメリット【その３】開発環境（すなわちコンテナ）を構築する過程は、すべて記述される。記述に従って、コンテナが構築されるので、開発に参加する全員が同じ開発環境となる

Windows/Macユーザーは、[Docker Desktop](https://www.docker.com/ja-jp/products/docker-desktop/)をインストールする

Windowsの場合は、Docker DesktopがWSLを利用するので、必ず先にWSLをインストールする

## 6. Visual Studio Code

Windows/Macユーザーは、[Visual Studio Code](https://code.visualstudio.com/)をインストールする

## 7. まとめ

自分専用のパソコンに、ターミナル、Git、Docker Desktop、Visual Studio Codeの4つに、ブラウザ（Google Chrome）を加えると、開発環境は完成する
