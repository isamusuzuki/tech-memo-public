# 新しいPCでGitHubを連携する

作成日 2021/07/18、更新日 2026/01/06

## 1. Gitがインストールされていることを確認する

```bash
git --version
# git version 2.52.0.windows.1
```

## 2. 自分の名前とメルアドを設定する

```bash
git config --global user.name "Taro Suzuki"
git config --global user.email taro@example.com

# 設定を確認する
git config --list
# => user.name=Taro Suzuki
# => user.email=taro@example.com
```

## 3. 公開鍵ファイル・秘密鍵ファイルを作成する

```bash
# ホームフォルダにいることを確認する
cd ~

# RSA暗号鍵を生成する
ssh-keygen -t rsa
# => 英語でいろいろ質問されるが、なにもいじらずにエンターキーを叩く
# => ファイル名もパスフレーズも指定しなくてオーケー
# => ~/.ssh フォルダに id_rsa と id_rsa.pub の2ファイルが生成される
```

## 4. GitHub に、自分の公開鍵を登録する

```bash
cd .ssh

# 公開鍵を表示する
cat id_rsa.pub
```

GitHub のページを開いてログインする ＞ ページ右上にある自分のアイコンをクリック ＞ Settings メニュー

Your Profile ページ ＞ 左枠 ＞ Access コーナー ＞ SSH and GPG keys

SSH Keys ページ ＞ 右上 ＞ "New SSH key"ボタン

- Titleに、新しいPCを示す名前を適当に入力する
- Keyに、コピペした公開鍵を貼りつける

"Add SSH key"ボタン

### 登録した公開鍵の鍵指紋を確認する

SSH Keysページの一覧のそれぞれ2行目に`SHA256:`で始まる文字列がある。これが鍵指紋

```bash
# 鍵指紋を表示する
ssh-keygen -l -f id_rsa
```

GitHubのページにある鍵指紋と表示した鍵指紋が同一であることを確認する

## 5. GitHubに、SSH接続できるようにする

GithubにSSH接続することは許可されていないが、SSH接続できるように設定しておかないと、Gitコマンドがちゃんと動かない

`~/.ssh/config`設定ファイルを作成する

```text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

GitHubにssh接続を試みて、設定が間違っていないことを確認する

```bash
ssh git@github.com
# PTY allocation request failed on channel 0
# ここでEnterキーを叩く
# Hi {username}! You've successfully authenticated, but GitHub does not provide shell access.
# Connection to github.com closed.

# ssh接続は許可されていないのでエラーとなるが、
# その次に表示される英語をよく見ると、自分の名前を呼びかけている
# SSH鍵が正しく機能しており、誰がアクセスしに来たのかを、GitHubが理解していることがわかる
```

## 6. GitHubからリポジトリをクローンする

- GitHubの目的のリポジトリのページを開く
- ページの真ん中よりも右上寄りにある"Code"ボタンをクリック
- ダイアログ登場 ＞ Cloneコーナー
- HTTPS, SSH, GitHub CLIのタブからSSHをクリック
- テキストボックスに表示されている呪文をコピーする
- `git clone` コマンドの後にその呪文を貼り付けて、実行する

```bash
cd ~
git clone git@github.com:{{YOURNAME}}/{{REPOSITORYNAME}}.git
```
