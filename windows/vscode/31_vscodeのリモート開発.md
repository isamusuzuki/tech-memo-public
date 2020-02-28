# vscode のリモート開発

作成日 2019/11/13、更新日 2019/11/27

## 01. リモート接続可能な Workspace

以下の 3 つの環境に、vscode からリモート接続して開発することができる

- Docker 上で動作している Workspace
- SSH で接続可能なリモートサーバーで動作している Workspace
- WSL で動作している Workspace

これで開発のための環境をローカルに構築しなくても、\
Docker と vscode がインストールしてあれば開発出来るようになる

vscode-remote-try で検索すると、\
いろんな言語のサンプルコードがあることがわかる \
=> [Search · org:Microsoft vscode\-remote\-try](https://github.com/search?q=org%3AMicrosoft+vscode-remote-try&unscoped_q=vscode-remote-try)

## 02. WSL を利用したリモート開発の実際

### 拡張機能を追加する

"Remote - WSL"をインストールする

[Remote \- WSL \- Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)

何が起こるかというと、WSL の環境で`code`コマンドを叩けるようになる

```bash=
which code
#=> /mnt/c/Program Files/Microsoft VS Code/bin/code
```

### ソースコードを WSL に置く

```bash=
cd ~
mkdir avocado
cd avocado
echo 'aaabbbccc' > test.txt
code .
# => Installing VS Code Server f06011ac164ae4dc8e753a3fe7f9549844d15e35
# => Downloading: 100%
# => Unpacking: 100%
```

code コマンドで開いた Visual Studio Code の違い

- エクスプローラーのフォルダ名に`[WSL]`と追記されている
- 左下に「WSL」と表示されている
- ターミナルを開くと、WSL の bash が登場する

### もうひとつの開始方法

- 左下の緑色部分をクリックするか、または、コマンドパレットで"Remote-WSL:New Window"を選ぶ
- 先にターミナルが開いている vscode ウインドウが登場する
- エクスプローラー上の「フォルダーを開く」ボタンは、WSL のフォルダだけを選べるようになっている

## 03. WSL2 になったら、なにが変わるのか

[Using WSL 2 with Visual Studio Code](https://code.visualstudio.com/blogs/2019/09/03/wsl2)

[Visual Studio Code と WSL 2 を使って開発してみよう](https://news.mynavi.jp/article/liunx_win-31/)
