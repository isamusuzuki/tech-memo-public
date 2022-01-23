# 新しい PC で GitHub を使えるように連携する

作成日 2021/07/18

## 00. 前提条件

- 新しい PC で 開発環境を整えたい
- GitHub アカウントはすでに持っている
- Ubuntu の Bash もしくは Windows 上の Git Bash が使える

※ Windows に Git アプリをインストールすると、Git Bash がついてくる

## 01. 新しい PC に、Git がインストールされていることを確認する

```bash
git --version
# => git version 2.25.1
```

もし Git が入っていなかったら、インストールする

```bash
sudo apt install git -y
```

## 02. 自分の名前とメルアドを設定する

```bash
git config --global user.name "Taro Suzuki"
git config --global user.email taro@example.com

# 設定を確認する
git config --list
# => user.name=Taro Suzuki
# => user.email=taro@example.com
```

## 03. 新しい PC で、公開鍵ファイル・秘密鍵ファイルを作成する

```bash
# ホームフォルダにいることを確認する
cd ~

# RSA暗号鍵を生成する
ssh-keygen -t rsa
# => 英語でいろいろ質問されるが、なにもいじらずにエンターキーを叩く
# => ファイル名もパスフレーズも指定しなくてオーケー
# => ~/.ssh フォルダに id_rsa と id_rsa.pub の2ファイルが生成される
```

## 04. GitHub に、自分の公開鍵を登録する

```bash
# 公開鍵を表示する
less .ssh/id_rsa.pub
# => コピペする、なんとかして自分の環境に持ってくる
```

GitHub のページを開いてログインする ＞ ページ右上にある自分のアイコンをクリック ＞ Settings メニュー

Your Profile ページ ＞ 左枠 ＞ Account Settings ＞ SSH and GPG keys

SSH Keys ページ ＞ 右枠 ＞ "New SSH key"ボタン

- Title に新しい PC を指し示す名前を適当に入力する
- Key にコピペした公開鍵をそのまま貼りつける

"Add SSH key"ボタン

### 登録した公開鍵の鍵指紋を確認する

SSH Keys ページの一覧のそれぞれ 2 行目に `SHA256:` で始まる文字列がある。これが鍵指紋

```bash
# 鍵指紋を表示する
ssh-keygen -l -f .ssh/id_rsa
```

GitHub のページにある鍵指紋と新しい PC で表示した鍵指紋が同一であることを確認する

## 05. 新しい PC から GitHub に、SSH 接続できるようにする

Github に SSH 接続することは許可されていないが、SSH 接続できるように設定しておかないと Gitコマンドがちゃんと動かない

`~/.ssh/config` 設定ファイルを作成する

```text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

GitHub に ssh 接続を試みて、設定が間違っていないことを確認する

```bash
ssh git@github.com
# => ssh接続は許可されていないので終了するが、
# => 表示される英語をよく読むと、自分の名前を呼びかけており、
# => 設定は間違っていないことがわかる
```

## 06. GitHub からリポジトリをクローンする

- GitHubの目的のリポジトリのページを開く
- ページの真ん中よりも右上寄りにある "Code" ボタンをクリック
- ダイアログ登場 ＞ Clone コーナー
- HTTPS, SSH, GitHub CLI の中の SSH を選ぶ
- テキストボックスに表示されている呪文をコピーする
- `git clone` コマンドの後にその呪文を貼り付けて、実行する

```bash
cd ~
git clone git@github.com:{{YOURNAME}}/{{REPOSITORYNAME}}.git
```
