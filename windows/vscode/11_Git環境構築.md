# vscode の Git 環境構築

作成日 2020/01/31

## 01. 事前に確認しておくべきこと

### Git がインストールされているか

なければ Chocolatey を使ってインストールする

[Chocolatey Software \| Git 2\.25\.0](https://chocolatey.org/packages/git)

### GitHub のアカウントを持っているか、ログインできるか

Git リポジトリはいろいろあるが、ここでは GitHub を使うこととする

### Visual Studio Code がインストールされているか

なければ公式サイトからインストーラーを入手する

[Visual Studio Code \- Code Editing\. Redefined](https://code.visualstudio.com/)

### Git Bash を起動できるか

Git Bash は、Git と一緒にインストールされるターミナルアプリ

## 02. 最初にやるべきこと

### Git Bash を起動する

まだ Visual Studio Code は必要ない\
以下は Git Bash ターミナルの中で実行する

### 自分の名前とメルアドを設定する

```bash
git config --global user.name "Taro Okamoto"
git config --global user.email taro@example.com
```

この名前は、コミットするときに一緒に書き込まれる\
GitHub のアカウントと同じである必要はない

### SSH 鍵を作成する

```bash
cd ~

# RSA暗号の鍵を生成する
ssh-keygen -t rsa
# => 3回エンターキーを叩く
# => 1回目は、保存するファイル名をデフォルトのままにするため
# => 2回目と3回目は、パスフレーズを空のまま登録するため
# => ~/.ssh/id_rsa, ~/.ssh/id_rsa.pub の 2つが出来上がる

# 公開鍵ファイルの中身を確認する
less .ssh/id_rsa.pub
# => Gitリポジトリの設定ページで、公開鍵ファイルの中身を登録する

# 鍵指紋を確認する
ssh-keygen -E md5 -l -f .ssh/id_rsa
# => 鍵指紋は、秘密鍵でも公開鍵でも同じ
# => Gitリポジトリの設定ページにも、鍵指紋が表示されている
```

### .ssh/config ファイルを作成する

```bash
cd ~
cd .ssh
touch config
vi config
```

`.ssh/config`ファイルの中身

```text
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
  Port 22
  TCPKeepAlive yes
  IdentitiesOnly yes
```

### ssh 接続を試みて、設定が間違っていないことを確認する

```bash
ssh git@github.com
# => ssh接続は許可されていないので失敗する（切断される）が、設定は間違っていないことがわかる
```

### git clone を実行する

```bash
cd ~
git clone git@github.com:<user-name>/<repository-name>.git
# => ホームフォルダに、リポジトリ名のフォルダが出来上がる
```

## 03. 該当のフォルダを、Visual Studio Codeで開く

### ターミナルを Git Bash に変更する

デフォルトは PowerShell になっているが、これを Git Bash に変更する

- 左下歯車アイコン ＞ settings ＞ settings ページ
- `terminal integrated shell windows`を検索
- `Edit in settings.json`リンクをクリック
- `settings.json`ファイルが開かれる（ユーザーレベル）
- 以下を書き込む

```json
{
  "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe"
}
```

## 04. コミットメッセージには、プレフィックスをつける

- `feat:` ... 新機能
- `fix:` ... バグフィックス
- `refactor:` ... 新機能でもバグフィックスでもないコード変更
- `perf:` ... パフォーマンス向上
- `test:` ... テストコードの追加・修正
- `style:` ... コードの意味に影響しない変更（空白、フォーマット、セミコロン）
- `docs:` ... ドキュメントだけの変更
- `chore:` ... 雑用（ビルドプロセスの変更、ツールやライブラリの追加削除）
